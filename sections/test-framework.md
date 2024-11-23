### Helm Chart Development
* Trino Community Kubernetes Helm Charts repository: https://github.com/trinodb/charts.git
* When developing new Helm Chart features there are some key aspects to consider:
    1. Does the feature work correctly?
    2. Is it robust (with respect to edge cases)? 
    3. Is it easy to use?

-vertical

### Test framework
* The Trino Helm Chart can be tested both locally and with GitHub Actions.
* To test it, the Chart must be installed on a K8s cluster.
* [kind](https://kind.sigs.k8s.io/) is used to create a K8s cluster running in a container.
* [chart-testing](https://github.com/helm/chart-testing) is used to install the chart and run tests.