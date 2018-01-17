---
id: 828
title: 'SOLID JavaScript: The Open/Closed Principle'
date: 2011-12-19T14:00:00+00:00
author: derekgreer
layout: post
guid: http://www.aspiringcraftsman.com/2011/12/19/solid-javascript-the-openclosed-principle/
permalink: /2011/12/19/solid-javascript-the-openclosed-principle/
dsq_thread_id:
  - "629994235"
categories:
  - Uncategorized
tags:
  - Design Principles
  - JavaScript
  - SOLID
---
<div style="border-bottom: black 1px solid; border-left: black 1px solid; float: right; margin-left: 4px; border-top: black 1px solid; border-right: black 1px solid" class="toc">
  <u>Series <noindex></noindex> Table of Contents</u> <br /><a href="/2011/12/08/solid-javascript-single-responsibility-principle/">The Single Responsibility Principle</a> <br />The Open/Closed Principle <br /><a href="/2011/12/31/solid-javascript-the-liskov-substitution-principle/">The Liskov Substitution Principle</a> <br /><a href="/2012/01/08/solid-javascript-the-interface-segregation-principle/">The Interface Segregation Principle</a> <br /><a href="/2012/01/22/solid-javascript-the-dependency-inversion-principle/">The Dependency Inversion Principle</a>
</div>

This is the second installment in the SOLID JavaScript series which explores the SOLID design principles within the context of the JavaScript language. In this installment, we’ll be covering the Open/Closed Principle.

<div style="clear: both">
  &#160;
</div>

## The Open/Closed Principle 

The Open/Closed Principle relates to the extensibility of objects. The principle states:

> <quote>Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification.</quote>

By _open for extension_, this principle means the ability for an entity to be adapted to meet the changing needs of an application. By _closed for modification_, this principle means that the adaptation of an entity should not result in modification of the entity’s source. More simply, entities which perform varied behavior should be designed to facilitate variability without the need for modification. Adherence to the Open/Closed Principle can help to improve maintainability by minimizing changes made to working code.

To help illustrate, let’s take a look at the following example which dynamically renders questions to a screen based upon the type of answer expected for each question:



&#160;

<div class="alt-display">
  &#160;
</div>

<div class="alt-display">
  <pre class="prettyprint">var AnswerType = {
    Choice: 0,
    Input: 1
};

function question(label, answerType, choices) {
    return {
        label: label,
        answerType: answerType,
        choices: choices
    };
}

var view = (function() {
    function renderQuestion(target, question) {
        var questionWrapper = document.createElement('div');
        questionWrapper.className = 'question';

        var questionLabel = document.createElement('div');
        questionLabel.className = 'question-label';
        var label = document.createTextNode(question.label);
        questionLabel.appendChild(label);

        var answer = document.createElement('div');
        answer.className = 'question-input';

        if (question.answerType === AnswerType.Choice) {
            var input = document.createElement('select');
            var len = question.choices.length;
            for (var i = 0; i &lt; len; i++) {
                var option = document.createElement('option');
                option.text = question.choices[i];
                option.value = question.choices[i];
                input.appendChild(option);
            }
        }
        else if (question.answerType === AnswerType.Input) {
            var input = document.createElement('input');
            input.type = 'text';
        }

        answer.appendChild(input);
        questionWrapper.appendChild(questionLabel);
        questionWrapper.appendChild(answer);
        target.appendChild(questionWrapper);
    }

    return {
        render: function(target, questions) {
            for (var i = 0; i &lt; questions.length; i++) {
                renderQuestion(target, questions[i]);
            };
        }
    };
})();

var questions = [
    question('Have you used tobacco products within the last 30 days?', AnswerType.Choice, ['Yes', 'No']),
    question('What medications are you currently using?',AnswerType.Input)
    ];

var questionRegion = document.getElementById('questions');
view.render(questionRegion, questions);</pre>
</div>

In this example, a view object contains a render method which renders questions based upon each type of question received. A question consists of a label, an answer type (choice or text entry), and an optional list of choices. If the answer type is Answer.Choice, a drop down is created with the options provided. If the answer type is AnswerType.Input, a simple text input is rendered.

Following the pattern already established, adding new input types would require adding new conditions within the render method. This would violate the Open/Closed Principle.

Let’s take a look at an alternate implementation that would allow us to extend the view object’s rendering capabilities without requiring changes to the view object for each new answer type :



&#160;

<div class="alt-display">
  <pre class="prettyprint">function questionCreator(spec, my) {
    var that = {};

    my = my || {};
    my.label = spec.label;

    my.renderInput = function() {
        throw "not implemented";
    };

    that.render = function(target) {
        var questionWrapper = document.createElement('div');
        questionWrapper.className = 'question';

        var questionLabel = document.createElement('div');
        questionLabel.className = 'question-label';
        var label = document.createTextNode(spec.label);
        questionLabel.appendChild(label);

        var answer = my.renderInput();

        questionWrapper.appendChild(questionLabel);
        questionWrapper.appendChild(answer);
        return questionWrapper;
    };

    return that;
}

function choiceQuestionCreator(spec) {

    var my = {},
        that = questionCreator(spec, my);

    my.renderInput = function() {
        var input = document.createElement('select');
        var len = spec.choices.length;
        for (var i = 0; i &lt; len; i++) {
            var option = document.createElement('option');
            option.text = spec.choices[i];
            option.value = spec.choices[i];
            input.appendChild(option);
        }

        return input;
    };

    return that;
}

function inputQuestionCreator(spec) {

    var my = {},
        that = questionCreator(spec, my);

    my.renderInput = function() {
        var input = document.createElement('input');
        input.type = 'text';
        return input;
    };

    return that;
}

var view = {
    render: function(target, questions) {
        for (var i = 0; i &lt; questions.length; i++) {
            target.appendChild(questions[i].render());
        }
    }
};

var questions = [
    choiceQuestionCreator({
    label: 'Have you used tobacco products within the last 30 days?',
    choices: ['Yes', 'No']
}),
    inputQuestionCreator({
    label: 'What medications are you currently using?'
})
    ];

var questionRegion = document.getElementById('questions');

view.render(questionRegion, questions);</pre>
</div>

There’s a few techniques being used here, so let’s walk through them one at a time.

First, we’ve factored out the code responsible for creating questions into a functional constructor named questionCreator. This constructor utilizes the <a href="http://en.wikipedia.org/wiki/Template_method_pattern" target="_blank">Template Method Pattern</a> for delegating the creation of each answer to extending types.

Second, we’ve replaced the use of the former constructor’s properties&#160; with a private spec property&#160; which serves as the questionCreator constructor’s interface. Since we’re encapsulating the rendering behavior with the data it operates upon, we no longer need these properties to be publicly accessible.

Third, we’ve identified the code which creates each answer type as a family of algorithms and factored out each algorithm into a separate object (a technique referred to as the the <a href="http://en.wikipedia.org/wiki/Strategy_pattern" target="_blank">Strategy Pattern</a>) which extends the questionCreator object using _differential inheritance_.

As an added benefit to this refactoring, we were able to eliminate the need for an AnswerType enumeration and we were able make the choices array a requirement specific to the choiceQuestionCreator interface.

The refactored version of the view object can now be cleanly extended by simply extending new questionCreator objects.

Next time, we’ll discuss the third principle in the SOLID acronym: The Liskov Substitution Principle.