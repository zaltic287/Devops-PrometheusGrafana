

# PromQL : Group by


* group by = identique à SQL sur la fonction

* opération de regroupement : count, sum...

* clause = by ()

* (cpu,instance)


--------------------------------------------------------------

# Exemples


<br>


* cas plus simple : node_cpu_seconds_total

* compter le nb de cpu

		* compter les lignes pas CPU

```
count(node_cpu_seconds_total) by (cpu)
count(node_cpu_seconds_total) by (cpu,instance)
```
		
		* compter le nombre de fois que l'on trouve le nb de lignes

```
count(count(node_cpu_seconds_total) by (cpu,instance))
count(count(node_cpu_seconds_total) by (cpu,instance)) by (instance)
```
