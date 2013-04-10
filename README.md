# SUnitToo(ls)-AutoTest

Automatically run appropriate tests when a method changes in Cincom
Visualworks Smalltalk.

SUnitToo(ls)-AutoTest is licensed under the MIT license.  See the
Copyright tab in the RB, the 'notice' property of this package, or the
License.txt file on GitHub.

SUnitToo(ls)-AutoTest's primary home is the
[Cincom Public Store Repository](http://www.cincomsmalltalk.com/CincomSmalltalkWiki/Public+Store+Repository).
Check there for the latest version.  It is also on
[GitHub](https://github.com/randycoulman/SUnitToo(ls)-AutoTest).

SUnitToo(ls)-AutoTest was developed in VW 7.9.1, but is compatible
with VW 7.7 and later.  If you find any incompatibilities with VW 7.7
or later, let me know (see below for contact information) or file an
issue on GitHub.

# Introduction

SUnitToo(ls)-AutoTest automatically runs relevant SUnitToo tests when
a method is changed.  The tests to run are selecting using the same
approach as the standard SUnitToo(ls) use.  If a test method is
changed, then that method will be run.  If a non-test method in a
TestCase subclass is changed, then all of the tests in that class will
be run.  If a method in a non-TestCase subclass is changed, then all
of the tests in the associated TestCase subclass (if any) will be run.

SUnitToo(ls)-AutoTest keeps track of a "hit count" for the changed
method.  That is, how many times the method was executed while running
the associated tests.  This allows you to see at a glance whether the
method you just changed is covered by the relevant tests.

# Usage

By default, SUnitToo(ls)-AutoTest is inactive when loaded into an
image.  In order to activate it, one or more AutoTest UIs must be
active.  SUnitToo(ls)-AutoTest currently has two UIs available:

* The main AutoTestUI: This UI runs in a separate window.  It can be
  opened by selecting `Tools` -> `AutoTest UI` in the Launcher.  This
  window displays the status of running or completed tests, as well as
  the hit count for the last changed method.  In addition, it allows
  you to cancel the current test run.  If any tests fail or raise
  errors, you can click on the test status portion of the window to
  browse the failing tests.  If only one test has failed or errored,
  the test will automatically be run again, opening a debugger to show
  the failure or error.

* The AutoTest Notifier (Linux only for now): The AutoTest Notifier
  does not show a window on the screen.  Rather, it integrates with
  your desktop environment's notification system to display a pop-up
  notification whenever a test run is complete.  The position,
  appearance, and duration of the notification can be configured using
  your desktop environment's settings.  To enable the AutoTest
  Notifier, select `Tools` -> `AutoTest Notifier` in the Launcher.

If you desire, you can run both AutoTest UI's at the same time.  To
disable AutoTest, simply close the AutoTest UI and disable the
AutoTest Notifier.  AutoTest will then be inactive until a UI is
opened again.

# Limitations

* SUnitToo(ls)-AutoTest does not run tests when class definitions are
  changed.  Class change notifications are triggered at some
  inopportune times (such as when loading code from Store), and so I
  removed the code that triggers test runs on class definition
  changes.

* SUnitToo(ls)-AutoTest does update the state of the method and class
  icons when run, but those changes are not immediately visible until
  some other action causes the browser to redisplay itself.  The
  browser does not provide a proper API for such a refresh.  There is
  an API that could be used, but it also causes any in-progress code
  changes to be lost, which is unacceptable.

* AutoTest Notifier is Linux-only at present.  If you are interested
  in an AutoTest Notifier for another platform, I'd be happy to accept
  contributions.  I will likely implement something for Mac OS/X when
  I have time.

* It is sometimes not safe to have SUnitToo(ls)-AutoTest active when
  working on some kinds of code, particularly DLLCC interfaces that
  are under active development.  Sometimes, running such a test at the
  wrong time can crash an image.  When working on such code, I
  recommend closing the AutoTest UI and disabling the AutoTest
  Notifier to deactivate AutoTest.

# Understanding the Code

`AutoTest` is the main class.  It is a singleton.  When active, it
listens for announcements from a `ChangeSetListener`, asks a
`BuildSuite` to create a test suite, and then runs it using
`SuiteRunner`.  `SuiteRunner` reports its progress by asking
`AutoTestAnnouncer` to announce various progress announcements.  a
`MethodHitCounter` (a `MethodWrapper`) is installed for the duration
of the test run to report a `HitCount` for the changed method.  An
`AutoTestResults` contains the final results of the test run.

# Acknowledgements

SUnitToo(ls)-AutoTest was inspired by the
[Pharo Autotest](http://www.squeaksource.com/Autotest.html) package by
Laurent Laffont.

# Contributing

I'm happy to receive bug fixes and improvements to this package.  If
you'd like to contribute, please publish your changes as a "branch"
(non-integer) version in the Public Store Repository and contact me as
outlined below to let me know.  I will merge your changes back into
the "trunk" as soon as I can review them.

# Contact Information

If you have any questions about SUnitToo(ls)-AutoTest and how to use
it, feel free to contact me.

* Web site: http://randycoulman.com
* Blog: Courageous Software (http://randycoulman.com/blog)
* E-mail: randy _at_ randycoulman _dot_ com
* Twitter: @randycoulman
* GitHub: randycoulman
