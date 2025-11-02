---
layout: page
title: Timetable
collection: projects
cover: /assets/timetable/cover.jpg
---

[Open Timetable](https://lucavicidomini.com/timetable/)
{:.text-center}

## Introduction

There are some middle schools in Italy where students attend regular classes in the morning. Then, in the afternoon, they split into groups to study a specific musical instrument.

After being "forced" several times by the coordinator to help fit all the schedules onto a single Word page, I finally decided to spend a couple of afternoons writing a tool to do it automatically.

The result goes under the very original name of **Timetable**.

## Features

- Runs entirely on browser.
- Support multiple pages.
- Each pages has a title.
- Each page has a grid with editable column headers (usually the days of the week), row headers (typically the time slots), and cells.
- A cell may contain several element separated by a blank line.
- Each element in a cell consists in two rows: tipically the first one is the student's name, the second one is the original class.
- Cells can be spanned on multiple rows.
- It is possible to define layout settings that apply to all pages.
- Page title, column headers, row headers, student name and class can be styled.
- Row and columns settings may have a background color.
- Empty cells may have a background color.
- Layout settings and pages are automatically saved in session's local storage, but can also be downloaded to a local file and imported later.

Please note that timetable solves a very specific request and is not meant to be a universal tools.

## Changelog

- 0.2.0
  - Allow to upload previously saved settings.
- 0.1.1
  - Fixed a bug with reactive forms that prevented using page settings.
- 0.1.0
  - First working prototype.

## Knows bugs

- Sometimes the content is overwritten when editing cells. This depends on the browser's local storage management, but it's being worked on.
- May not be able to fit a lot of students in a single cell.

## Technical specs

- Angular 19
- Signals
- Reactive forms
- [jsPDF](https://github.com/parallax/jsPDF)