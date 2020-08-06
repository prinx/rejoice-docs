---
title: Introduction
layout: default
nav_order: 1
---
# THE REJOICE FRAMEWORK
## Introduction
Rejoice is a PHP framework for USSD applications. It main goal is to speed up your USSD application development while having at anytime a scalable code.

The Rejoice Framework allows you to create simple to complex USSD applications by allowing you to create each menu and have control over everything from what is displayed to controling the users inputs.

## What is USSD?
USSD stands for **Unstructured Supplementary Service Data**. It is a network protocol allowing mobile operators to send information and communicate with phone users.

A USSD application is basically a series of popups (which are called *menus* or *pages* or *screens*) that display or request information from the user via their mobile phones.

## How does it works?
Unlike a normal web application, a USSD request is created when a user dials a USSD code on their phone number. A USSD code is usually starts with a star (`*`) and ends with a hash (`#`). The request is forward to the mobile operator. The mobile operator then either process the request itself, if the shortcode maps to an application developed (with Rejoice) by the mobile operator itself. Or it forwards the request to a third-party's application (again, developed with Rejoice).

A USSD request is a stateful request. The interaction User-application happens in a session automatically created when the user firstly dialed the USSD code, which ends when either the user has completed the USSD flow or when they decide to cancel the session themselves.

## What do I need to know before starting?
You don't need to know anything apart from the Object-Oriented PHP language. The rest we will guide you step-by-step to make your best application.

## Our Philosophy
You must be shouting for joy while building your application 😀

<div class="d-flex flex-justify-end">
<a href="current/installation.html" class="btn">Getting Started</a>
</div>