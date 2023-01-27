# D2IT2 research and ideas

For the D2I Tool 2 we want to look at:

* As an analyst, you can develop visualisation and analysis in a familiar
  framework without having to worry about the end-user experience

* Using a simple "tool description format",  you can express the expected user
  experience without having to be familiar with front-end tools

* To share your tool, you can submit to D2I for inclusion in the directory of
  tools. This will allow others to re-use your analysis in a secure environment.

The [Demand Modelling][dm] tool uses a proof-of-concept of this approach where
the python code returns a state that also includes a description of the UI
to be rendered.

There are several avenues we want to explore in terms of the technology stack:

## Code extraction

How do we allow analysts to develop code in a user friendly way. This will
most likely mean it has the following features:

* A familiar user interface
* Access to secure data so transformations and charts can be tested
* Ability to preview the app
* Should not make it easy to (accidentally or intentionally) include
  data in exported scripts

[Read more](./docs/code-export)



[dm]: https://github.com/data-to-insight/cs-demand-model
