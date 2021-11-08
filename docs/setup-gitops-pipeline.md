# OS-Climate Data Commons Developer Guide: Setup GitOps pipeline

This section needs to define how developers can name and use pipelines for
* Development on their own branch (or the branch that results from accepting a pull request)
* CD/CI testing (we don't want automated testing to break every branch that every developer is working on)
* General release to the Data Commons community (it's fine to have a canonical name mean "latest", but releases also need to be versioned)

Presently developers are using canonical release names in their notesbooks (such as `osc_datacommons_dev.gleif`), which is all well and good for a single developer working by themselves.  But really there needs to be some way for developers to tag their intended canonical name with some additional information so that the overall system can do the right thing when multiple developers and/or release engineers are all working together in a variety of environments that are all connected.  Now that we have several ingestion streams with multiple developers working (or wanting to work on them), this needs to be defined and implemented as a library that all developers of ingestion pipelines (as well as all users of ingestion pipelines) can use.

The currently-defined `osc_datacommons_dev`, `osc_datacommons_test`, and whatever we call the OS-C production environment does not provide enough granularity, and expecting developers to manually tweak strings in notebooks is asking for trouble.
