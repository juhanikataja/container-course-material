# Cloud native apps

---

## Change of mindset needed

* Using Kubernetes/OpenShift is very different compared to plain VMs
* You may need to let go of certain old habits

---

## Priciples

* Everything is code
* Maximize similarity between dev and prod environments
* Clearly defined pipeline from dev to prod
* Explicit dependencies
* Separate state from configuration

---

## Keep in mind

* Pods are ephemeral and are meant to die
* Pod storage is also ephemeral unless you use volumes
* Separate stateful and stateless parts of your application

---

## 12 factor apps

* [https://12factor.net/](https://12factor.net/)
* 12 guidelines for developing cloud native applications
* Makes it easier to deal with modern application development
* Also see [12 Fractured Apps](https://medium.com/@kelseyhightower/12-fractured-apps-1080c73d481c)
  from Kelsey Hightower for some practical tips on getting closer to 12 factor

Note:

12 Factor is made by Adam Wiggins and is licensed under the MIT license:

Copyright (c) 2012 Adam Wiggins

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

---

* **I. Codebase** - One codebase tracked in revision control, many deploys

* **II. Dependencies** - Explicitly declare and isolate dependencies

* **III. Config** - Store config in the environment

Note:

* Codebase
   * One codebase is one app, several would be a distributed system
   * The same codebase should not be used for multiple apps
   * Different versions of the same codebase may be active in different deployments
* Dependencies
   * Don't rely implicitly on the existence of system wide packages
   * All dependencies are declared to the level of which version of a dependency to
     get
   * Ensure no dependencies leak in from the system to the app
   * Examples: Gemfile (Ruby), Pip+Virtualenv (Python), Docker
* Config
   * Config is everything that is likely to vary between deploys
   * Strict separation between config and code
   * Can you make the codebase open source at any moment without divulging
     confidential data?

---

* **IV. Backing services** - Treat backing services as attached resources

* **V. Build, release, run** - Strictly separate build and run stages

* **VI. Processes** - Execute the app as one or more stateless processes

Note:

* Backing services
   * Anything consumed over the network as part of normal ops is a
     *backing service*
   * Local (e.g. DB) and third party backing services (e.g. SMTP service,
     monitoring and metrics services)
   * *There should be no distinction between local and third party services in
     the code*
   * Should be possible to change between local and third party backing services
     without changing the code
* Build, release, run
   * Build: bundle the app and its dependencies
   * Release: combine the build with a config to end up with a ready to run
     release
   * Run: run the app in the execution environment
   * Strict separation between different stages
   * Releases have a unique identifier
* Processes
   * Processes are stateless and share-nothing
   * Any data that needs to persist is stored in a stateful backing service
   * Don't store a given user's data in a single process and expect further
     requests from that user to go to the same process

---

* **VII. Port binding** - Export services via port binding

* **VIII. Concurrency** - Scale out via the process model

* **IX. Disposability** - Maximize robustness with fast startup and graceful shutdown

Note:

* Port binding
   * The app is self-contained and doesn't rely on a separate web server that
     gets added at runtime
   * The app exposes HTTP as a service by binding to a port
   * Typically you add a webserver library to the app at build time
* Concurrency
   * Processes are first class citizens
   * Different types of work are assigned to different process types
   * Scale out by creating more processes to handle additional work
* Disposability
   * Processes are disposable and can be started and stopped quickly
   * Shut down gracefully when you get a SIGTERM: stop listening and handle any
     ongoing requests
   * Worker jobs are reentrant by being wrapped in a transaction or by being
     idempotent
   * Processes are robust against sudden death

---

* **X. Dev/prod parity** - Keep development, staging, and production as similar as possible

* **XI. Logs** - Treat logs as event streams

* **XII. Admin processes** - Run admin/management tasks as one-off processes

Note:

* Dev/prod parity
   * Time gap: code takes a long time to go from dev to prod
   * Personnel gap: different people handle dev and ops
   * Tools gap: different tools in dev vs. production
   * *Continuous deployment through keeping the gap between dev and prod small*
   * 12 factor: same people develop and deploy apps continuously, using the same
     tools as much as possible in development and production
   * Example: Don't use SQlite in dev while using PostgreSQL in production
* Logs
   * 12 factor apps don't write logfiles - they output logs to `stdout`
   * The execution environment handles capture and routing of log streams
   * The execution environment routes logs to a file and/or sends them to a
     separate log indexing and analysis system
* Admin processes
   * Admin processes are one-off tasks like DB migration, running a console for
     the app or one-time scripts in the apps repo
   * Should run in the same execution environment as the app
