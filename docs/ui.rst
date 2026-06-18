..
   Licensed to the Apache Software Foundation (ASF) under one
   or more contributor license agreements.  See the NOTICE file
   distributed with this work for additional information
   regarding copyright ownership.  The ASF licenses this file
   to you under the Apache License, Version 2.0 (the
   "License"); you may not use this file except in compliance
   with the License.  You may obtain a copy of the License at

     http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing,
   software distributed under the License is distributed on an
   "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
   KIND, either express or implied.  See the License for the
   specific language governing permissions and limitations
   under the License.


.. _ui:

=======
Burr UI
=======

Burr comes with an open-source telemetry UI for monitoring, debugging, and replaying
your application runs in real time. It works locally out of the box and can also be
deployed alongside your production stack.

.. image:: _static/chatbot.png
    :alt: Burr UI showing a chatbot application graph
    :align: center

-----------
Quick Start
-----------

Install Burr with the UI extras and launch the server:

.. code-block:: bash

    pip install "apache-burr[start]"
    burr

This starts the tracking server on port ``7241`` and opens the UI in your browser.
Any application that uses :py:meth:`with_tracker <burr.core.application.ApplicationBuilder.with_tracker>`
will automatically appear in the UI.

.. code-block:: python

    app = (
        ApplicationBuilder()
        .with_actions(...)
        .with_transitions(...)
        .with_tracker("local", project="my-project")
        .build()
    )

----------
Data Model
----------

The UI is organized around three levels:

1. **Projects** — the top-level grouping, set via the ``project`` argument to ``with_tracker``.
2. **Applications** — individual runs logged to a project, similar to a "trace" in distributed tracing. Set via the ``app_id`` argument.
3. **Steps** — each action executed in the state machine. The UI shows the state, inputs, and results at every step.

------------------
Notebook / Colab
------------------

Launch the UI from a Jupyter notebook or Google Colab using the ``%burr_ui`` IPython magic:

.. code-block:: python

    # Expose the port and print the URL
    %load_ext burr.integrations.notebook
    %burr_ui
    # → "Burr UI: http://127.0.0.1:7241"

For Google Colab, forward the port to the browser:

.. code-block:: python

    from google.colab import output
    output.serve_kernel_port_as_window(7241)   # opens a new window
    output.serve_kernel_port_as_iframe(7241)   # inline iframe

--------------------------
Embed in a FastAPI App
--------------------------

Mount the Burr UI inside an existing FastAPI application:

.. code-block:: python

    from fastapi import FastAPI
    from burr.tracking.server.run import mount_burr_ui

    app = FastAPI()
    mount_burr_ui(app, path="/burr")

The tracking UI will then be available at ``/burr`` on your existing server.

--------
See Also
--------

- :ref:`tracking` — full technical reference for the tracking client, data storage, and state replay
- :ref:`Additional Visibility <opentelref>` — tracing, OpenTelemetry spans, and LLM instrumentation inside actions
- :doc:`examples/deployment/monitoring` — deploying the tracking server in production (local, S3, Docker)
