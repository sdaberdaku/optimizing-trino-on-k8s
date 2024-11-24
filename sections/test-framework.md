### Helm Chart Development
* Trino Community Kubernetes Helm Charts repository: https://github.com/trinodb/charts.git
* When developing new Helm Chart features there are some key aspects to consider:
    1. Does the feature work correctly?
    2. Is it robust (with respect to edge cases)? 
    3. Is it easy to use?

-notes
Trino configuration can be quite complex, and it is important that the Helm Chart does not simply end up adding another layer of complexity. 

Automated testing is an important aspect to consider when developing new features for the Helm Chart, since it is the only way to ensure correctness even in edge cases. 

-vertical

### Test framework
* The Trino Helm Chart can be tested both locally and with GitHub Actions.
* To test it, the Chart must be installed on a K8s cluster.
* [kind](https://kind.sigs.k8s.io/) is used to create a K8s cluster running in a container.
* [chart-testing](https://github.com/helm/chart-testing) is used to install the chart and run tests.