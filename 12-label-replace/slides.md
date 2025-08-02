

# PromQL : Manipuler les labels

<br>


* label : élément pour filtrer et typer des métriques (très utiles aussi pour grafana)

* éviter de collecter/stocker trop de métriques

* relabel : https://medium.com/quiq-blog/prometheus-relabeling-tricks-6ae62c56cbda

* doc auto découverte : https://prometheus.io/docs/prometheus/latest/configuration/configuration/

----------------------------------------------------------------------------------

# relabel_configs : action sur un scrape


* toutes les métriques d'un filtre

<br>


* drop de noeuds sur un label (adresse)

```
scrape_configs:
  - job_name: prometheus
    static_configs:
      - targets: ['localhost:9090']
  - job_name: node_exporter
    static_configs:
      - targets: ['192.168.59.3:9100']
    relabel_configs:
      - source_labels: [__address__]
        regex: '192.168(.*)'
        action: drop
```

<br>


* keep

```
    relabel_configs:
      - source_labels: [__address__]
        regex: '192.168.59.2'
        action: keep
```

-----------------------------------------------------------------------------------


# metric_relabel_configs : action sur une métrique

<br>


* garder que quelques métriques

```
    metric_relabel_configs:
      - source_labels: [ __name__ ]
        regex: '(node_load1|node_load15)'
        action: keep
```


<br>


* relabel : exemple réédition de ports

```
    relabel_configs:
      - source_labels: [__meta_consul_tags]
        regex: .*,metrics,.*
        action: keep
      - source_labels: ['__address__']
        separator:     ':'
        regex:         '(.*):(.*)'
        target_label:  '__address__'
        replacement:   '${1}:8080'
```


----------------------------------------------------------------------------------

# PromQL : par fonction 


* fonction label_replace

```
label_replace(v instant-vector, dst_label string, replacement string, src_label string, regex string) 
```

<br>



* exemple :

```
label_replace(up, "mylabel", "$1", "job", "(.*)")
```
