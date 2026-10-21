# Optimizing Trino on Kubernetes

Slides for the talk "Optimizing Trino on Kubernetes: Helm chart enhancements for resilience and security", given by
Sebastian Daberdaku (Cardo AI) and Jan Waś (Starburst) at Trino Summit 2024 (December 11-12, 2024).

- Slides: [https://sdaberdaku.github.io/optimizing-trino-on-k8s/](https://sdaberdaku.github.io/optimizing-trino-on-k8s/)
- Recording: [https://youtu.be/MGuOf45cGwA](https://youtu.be/MGuOf45cGwA)
- Trino Summit 2024 recap: [https://trino.io/blog/2024/12/18/trino-summit-2024-quick-recap.html](https://trino.io/blog/2024/12/18/trino-summit-2024-quick-recap.html)

The talk covers changes to the community Trino Helm chart: optional default catalogs, templating, worker graceful
shutdown, the worker JMX exporter and network policies. The last section is future work on worker autoscaling with KEDA.
The slide sources are the Markdown files in `sections/`.

## Presenting

Run a local webserver with the `runLocalServer.sh` script, and navigate to
[http://localhost:9000](http://localhost:9000). The script takes an optional port as its first argument and uses
python3, ruby or busybox, whichever it finds first.

Press `?` for keyboard shortcuts, and find more information in the [reveal.js
documentation](https://revealjs.com/).

## More information

- Uses [reveal.js](https://revealjs.com/) version 4.4.0 source in the `reveal`
  folder.
- [reveal.js documentation](https://github.com/hakimel/reveal.js/) for info on
  writing and other details.
- [keyboard shortcuts](https://github.com/hakimel/reveal.js/wiki/Keyboard-Shortcuts)
  for presenting
