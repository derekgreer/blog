---
id: 1047
title: "Adventures in Debugging: The NHibernate 'don't flush the Session' Error"
date: 2013-01-27T15:49:37+00:00
author: derekgreer
layout: post
guid: http://aspiringcraftsman.com/?p=1047
permalink: /2013/01/27/adventures-in-debugging-the-nhibernate-dont-flush-the-session-error/
dsq_thread_id:
  - "1050113156"
categories:
  - Uncategorized
tags:
  - Debugging
---
This <noindex></noindex> is the first article in a new sporadic series I’ll contribute to from time to time wherein I’ll discuss some noteworthy issues I’ve wrestled with. In this installment, I’ll be discussing an NHibernate issue which took me some time to work through. So, let’s dive into the story &#8230; 

## The Context

In an application I was recently working on, a need arose to modify a section of code involving two entities which should have been modeled using a parent/child relationship but which only had a loose association in the database. The primary table in the database schema for what needed to be the parent object in the domain only contained a unenforced foreign key column which matched up with a candidate key on the table used for what needed to be the child object. In the section of code I needed to modify, a View Model was being created by first retrieving data for the parent object and subsequently for the child object. I’m not exactly sure what lead to this path, but I think it had something to do with the original developer’s attempt at using a surrogate key strategy for all the tables and later attempts by others to pull the data into a domain model with NHibernate. 

At any rate, while I wasn’t in a position to revamp the whole design, I knew there was a way to express many-to-one mappings in NHibernate using non-primary keys, so after a little searching and some trial-and-error I got the parent entity referencing the child entity with a Fluent NHibernate Auto-Mapping configuration similar to the following: 

<pre class="prettyprint">return AutoMap.AssemblyOf&lt;parententity>(new AutomappingConfiguration())
  .Override&lt;parententity>(map =&gt; map.References(p =&gt; p.Child, 
  "ParentColumnChildKeyName").PropertyRef("ChildCandidateKeyColumnName")
    .Fetch.Join());</pre>

&#160;

Part of the changes required to make this work was some refactoring of an import job used to populate the database which relied upon the domain model and mappings to populate the parent and child data. After changing the parent entity to reference the child entity instead of just a candidate key to the child entity, I needed to modify the import job to persist the relationship between the parent and the child. To do this, I injected a pre-existing ChildRepository to query for existing instances of the child entities (which had its own separate import process) so I could associate it with the parent entity upon saving. All of the changes worked as expected for the client portion of the application, but the changes broke some acceptance tests for the import job. The error I started receiving in the tests was as follows: 

<pre class="prettyprint">null id in “MyEntityType” entry (don't flush the Session after an exception occurs)</pre>

In this case the “MyEntityType” was another entity which had a many-to-one mapping with the aforementioned parent entity. After looking over the code and scratching my head for a bit, I decided to do a search on this particular error and read a few articles which at first didn’t seem to speak to my particular scenario. The advice I read basically boiled down to “Don’t try to do stuff with the session after you receive an error”. That certainly made sense, but upon stepping through the code I couldn’t see anywhere I was catching an error and proceeding to do something further with the session. I then decided to add a try/catch around the offending code and suddenly I saw the issue: trying to save an entity associated with one open session with an entity from another open session. 

## The Solution 

Ultimately, the reason I couldn&#8217;t see the error was due to an issue with a manifestation of some common infrastructure code my team uses when working working with NHibernate. We use Autofac for dependency injection, and to facilitate transactions we use Autofac’s OnActivating() and OnRelease() methods to begin an NHibernate transaction and to handle the rollback or commit of the transaction when complete. Here was the offending code: 

<pre class="prettyprint">builder.Register(c =&gt; c.ResolveNamed&lt;isessionfactory>(RegistrationKey).OpenSession())
	.As&lt;isession>()
	.OnActivating(x =&gt; x.Instance.BeginTransaction())
	.OnRelease(session =&gt;
		{
			try
			{
				if (!session.Transaction.WasRolledBack && session.Transaction.IsActive)
				{				
					session.Transaction.Commit();
				}
			}
			finally
			{
				session.Close();
				session.Dispose();
			}
		});</pre>

When used within the context of our Web applications, this code would contain a call to register the ISession with an HTTP Request lifetime scope, but this import job didn’t require a shared ISession prior to my changes. To fix the problem, I added a call to register the ISession as InstancePerLifetimeScope() which causes the same lifetime scope used to resolve the job to be used for resolving any instances of ISession. Additionally, I added a try/catch/throw around the session to at least provide some logging of similar issues should this ever come up again.
