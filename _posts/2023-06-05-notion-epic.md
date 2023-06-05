---
layout: post
title:  "Notion Epic template"
date:   2023-06-05
excerpt_separator: <!--more-->
---
We are using [Notion](https://www.notion.so/) for knowledge sharing and task management at work.

We use the Engineering Roadmap template with several minor customizations to track our development processes.

Today I'll show you how I improved the Epic type of tickets to be more descriptive and demonstrate the crucial data quickly for the viewers. <!--more-->

### Epic expectations

For me, an epic represents a goal I want to achieve. It collects all the small steps (in the Roadmap template, it's called tasks) that I need to complete to reach the goal defined by the epic.

As a manager, I must focus more on the big picture than on individual tasks. For example: how many of them are completed already, which items are in our current focus, etc.

But in the default epic, we only have a `Tasks` field, which lists nothing, but the task names. No statuses, assignees, or other custom attributes I want to see.

### Linked view of a database

The inspiration came from Jira, where you can see the progress of the subtasks in the parent task.

1. I created a new block in my epic: a `linked view of database`
2. Add the Roadmap as the data source
3. Create a new table view with the name `Tasks`
4. Hide the database title, as I know precisely the database I'm in
5. Show properties: Title, Status, Assignee, Type, Updated
6. Filter: Epic is my current epic
7. Sort: Updated descending
8. After finishing the view, I ordered the columns to be more informative: Title, Status, Assignee, Type, Updated.

<img src="https://matevojts.github.io//assets/images/notion_epic_template.jpg" style="display: block; margin: auto;" />

This view solved my original issue: I can quickly get the summarized status of the epic and its individual tasks on one page.

### Repetition

I'd suggest creating a template from the epic so you don't need to reimplement the wheel next time.

[Official docs how to do it.]( https://www.notion.so/help/database-templates)

### Conclusion

A linked view of a database is a powerful tool where you can filter, sort, and display tasks that best fit your goal.

Are you also a power user of it?
