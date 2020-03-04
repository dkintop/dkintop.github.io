---
layout: post
title:      "Post Graduation Project"
date:       2020-03-03 15:01:48 -0500
permalink:  post_graduation_project
---


Following graduation from Flatiron School's Software Engineering bootcamp, I find myself thinking "what next?" quite often. I know there is an endless amount of new technologies to learn and all kinds of problems to be solved through software, but where do I start? What do I focus on next? That is the problem I have been struggling with as I find myself using my study time simply wandering the internet trying to find something to focus on. I realized that the best way to truly continue to learn is going to be focusing on a project and making the effort to attempt thing's out of neccessityrather than just wanting to learn them.

I utilized my past career experience as a Physical Therapist Assistant(PTA) as a starting point. What problems did I experience as a PTA that could have been solved or made simpler by a software application? I remembered always having to perform what is known as 'standardized testing' in order to quantify patient progress, since the insurance companies paying for the physical therapy services required it. I remembered having to write by hand on an outdated form the scores that patients would achieve on these tests and then calculate a score, by hand. There are dozens of tests out there all with their own sets of rules on how to score them and how to calculate scores, and all too often I knew of situations where scores were not being recored or calculated accurately due to user error. I figured this would be a good problem to attempt to tackle, but how?

In order to make this aspect of the PTA's job simpler and more effective I decided the application to solve this problem would need:
* user friendly and responsive UI
* allow for accurate recording of outcomes and display calculated results
* tests must be repeatable and changes in score must be tracked over time.
* multiple test types need to be accessible, all with their own scoring methods maintained
* ability to generate a PDF with score results.

In order to accomplish this functionality, I know I will need features that include:

* facility accounts, user accounts that belong to a facility, and patient profiles(patient profiles stored in database will not be hippa compliant in development build, this is one aspect that will need to be addressed if application were to go into production phase.)
* facility accounts would need an admin panel at some point to add users to facility(perhaps user accounts can only be created through the admin panel of the facility?)
* a feature to convert an html document into a pdf
* a feature that compiles all patient data and presents in a logical and visually appealing way to monitor score progress.
* a simple way for users to select a patient, a test to administer,  and then be presented with a form to easily input test scores and generate a result based on the individual tests scoring parameters.

I plan to build this project using a back end rails api to manage the databases and using a React front end to create a simple yet appealing UI that people will want to use. There is a lot of knowledge that I will need to seek out in order to develop this as there are several features I have no experience in implementing, But I think it will give me a great  learning experience and will be a great portfolio piece once finished!






