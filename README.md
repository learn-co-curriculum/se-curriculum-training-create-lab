# Creating a Lab (Assignment)

## Learning Goals

- Create an assignment in Canvas with the contents of the GitHub repository
- Create a `solution` branch

## Introduction

In the last couple lessons, we saw how to work with **pages** in Canvas for
creating simple lessons that just involved reading. In this lesson, we'll see
how to create **assignments** in Canvas, and discuss how students work through
applied learning exercises by writing code and running tests.

## Lab Overview

All of the labs we have in the curriculum use the "assignment" type in Canvas.
The main thing that differentiates a "page" from an "assignment" is that an
assignment has some element that students must submit in order to receive credit
for their work.

Our labs are used as a form of applied learning. Labs should generally follow a
readme and give students a chance to practice what they've learned by writing
code. Our labs all encourage a test-driven development workflow: the curriculum
team writes a set of deliverables for each lab, and also writes tests that check
whether a student has completed each deliverable.

The student workflow for each assignment is to:

- Fork the lesson's GitHub repository
- Clone the repository
- Run the tests
- Write code until all the tests are passing
- Submit their forked lesson in Canvas

Students use the [learn-test][] Ruby gem to automate a number of these steps.

From the curriculum team's point of view, what's important is that every lab
has:

- Clear instructions on the lab's deliverables
- Any starter code needed
- A set of tests written in a framework that the [learn-test][] gem understands
- A solution branch with a working solution

All of that is to say, creating labs is a bit more involved than creating
readmes!

## Creating a Lab

Start by making a new directory for your lesson (replacing `your-name` with your
name) and initializing Git:

```console
$ mkdir se-curriculum-training-your-name-lab
$ cd $_
$ git init
```

The code you'll need for a lab will depend on the type of lab you're creating.
You can find some lab templates in the [curriculum templates][] repository. If
you're creating a lab in an existing course, you can also find an existing lab
to use as a starting point.

For our example lab, let's use the [javascript-lab template][]:

- [https://github.com/learn-co-curriculum/se-curriculum-templates/tree/main/javascript-lab][javascript-lab template]

Copy all the files and folders from this template into your
`se-curriculum-training-your-name-lab` directory.

Commit your changes:

```console
$ git add .
$ git commit -m 'Initialize lab'
```

## Creating a Lab Repository

The next few steps will feel familiar. As with the readme lesson, we can use the
GitHub CLI to create a new remote repository and push up our code. Create the
repo on GitHub:

```console
$ gh repo create learn-co-curriculum/${PWD##*/} --public --source=. --push
```

We can view our new repository on GitHub with this command:

```console
$ gh repo view -w
```

## Creating the Assignment in Canvas

Now that we've created the lesson, it's time to push it up to Canvas using the
github-to-canvas gem!

Run the following command from the `se-curriculum-training-your-name-lab`
directory:

```console
$ github-to-canvas --create-lesson 5465 -lr --type assignment --forkable
```

As before, if all goes well, you should see something like:

```txt
Canvas lesson created. Lesson available at https://learning.flatironschool.com/courses/5465/assignments/164211
```

Head to the link in Canvas and verify that our lesson was created successfully.
Sweet!

Let's discuss what's changed from the readme version of this command:

- `--type assignment`: specifies that the type of lesson is a assignment rather
  than an page, since students are expected to submit their code for this
  lesson. If the type isn't specified, the gem will infer it based on the folder
  structure. It's safest to specify a type directly, though.
- `--forkable`: this creates a blue "FORK" button in the upper-right corner of
  the lesson. It's **crucial** that this button exists for all assignments, as a
  requirement for the `learn-test` gem to work properly when students interact
  with the lesson.

Make a new commit for the `.canvas` file, and push it up:

```console
$ git add .canvas
$ git commit -m 'Add .canvas file'
$ git push
```

## Adding a Lesson to a Module

Just like last time, we'll be adding our newly created lesson to the "Lessons
Created By Staff" module:

- Return to the [course homepage][] in Canvas
- Click the "+" button in the "Lessons Created By Staff" module
- From the dropdown, select "Assignment"
- Select your lesson
- Click "Add Item"

We'll also add a [module requirement][] for the lesson. For assignments, we
require that students submit the assignment in order for it to be marked as
complete. To add a module requirement:

- Click the three dots in the "Lessons Created By Staff" module
- Select "Edit"
- Click "Add requirement"
- From the new requirement, select your lesson, and select "submit the
  assignment" as the requirement
- Click "Update Module"

Now when a student submits the lesson using the `learn-test` gem, it will be
marked as complete.

You can verify that this worked successfully by checking if "Submit" is listed
under the lesson title in the module.

Nice work!

## Reviewing Assignment Settings

When the github-to-canvas gem creates a new lesson in Canvas, it sets a few
default settings for the assignment that impact grading and assignment
submission. These settings can be viewed by going to the assignment page and
clicking the "Edit" button.

![Canvas assignment settings](https://curriculum-content.s3.amazonaws.com/curriculum-training/create-lab/canvas-assignment-settings.png)

- **Points**: 1
- **Assignment Group**: (see below)
- **Display Grade as**: Complete/Incomplete
- **Submission Type**: Online / Website URL

Our courses have the following assignment groups:

- **Blog**: each phase has one blogging assignment for the Live program where
  students can submit links to their blogs for that phase.
- **Labs**: all homework labs.
- **Quizzes**: all quizzes.
- **Code Challenges**: all code challenges (there should be four per phase).
- **Project Review**: all assignments related to the final project for the
  phase.
- **Misc**: all assignments that don't fit in any of the above categories.

You can change the assignment group to "Lab" to keep it organized.

## Creating a Solution Branch

Every lab must have a solution branch that contains working solution code. This
is helpful both to validate that the tests you've written for the lab work, and
to provide a place for students to check their work.

Take a look at the tests provided for this lesson in the template:

```js
// tests/index.test.js
describe("todo", () => {
  it("todo", () => {
    expect(aVariable).to.equal("hello");
  });
});
```

This is just an example of what a test could look like (you'd obviously need to
change the `"todo"`s to be descriptive and write more tests based on the
learning objectives of the lab), but it gives us enough to work on and create a
solution branch.

Create a new `solution` branch off the `main` branch:

```console
$ git checkout -b solution
```

Now add your solution code:

```js
// index.js
const aVariable = "hello";
```

Verify that your solution is working:

```console
$ npm install
$ npm test
```

Once you have a working solution, commit your changes:

```console
$ git add index.js
$ git commit -m 'Add solution code'
```

Your local repository should now have both required branches:

- `main` has the starter code students need to complete the lesson
- `solution` has the completed code that passes all the tests and meets all the
  deliverables

You'll need to push the `solution` branch to GitHub as well:

```console
$ git push -u origin solution
```

## Conclusion

Our first lab is ready to go! We've got the repo set up with a `main` and
`solution` branch, and the assignment set up in Canvas. Onwards to the next
task!

[learn-test]: https://github.com/learn-co/learn-test
[course homepage]: https://learning.flatironschool.com/courses/5465
[module requirement]:
  https://community.canvaslms.com/t5/Instructor-Guide/How-do-I-add-requirements-to-a-module/ta-p/1131
[curriculum templates]:
  https://github.com/learn-co-curriculum/se-curriculum-templates
[javascript-lab template]:
  https://github.com/learn-co-curriculum/se-curriculum-templates/tree/main/javascript-lab
