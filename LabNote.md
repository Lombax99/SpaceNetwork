
```

config file:
	/etc/dtnme_deamon.cfg
	
ssh connection.
	ips: student@10.0.0.11 [12, 13, 14]		pass: foobar
	
run dtme:
	cd ion
	sudo dtme -d -o ./dtn.log -t		#-t clear the database data, fresh start
	
connect to dtnme demon (if it's running):
	telnet localhost 5050
	
command while in the demon:
	link dump							#fa vedere le connessioni
	route dump							#fa vedere le route
	link add [link name]						#crea un nuovo link
	link close [link name]						#chiude un link esistente
	link open [link name]						#apre un link chiuso
	
dtnperf tool:
	cd ion
	sudo dtnme -d -o ./dtn.log -t					#attiva il demone
	dtnperf_vDTN_vBLABLA... --monitor				#attiva il nodo come monitor
	dtnperf_vDTN_vBLABLA... --server									#attiva il nodo come server in ascolto
	dtnperf_vDTN_vBLABLA... --client -d [destination node escluso "/dtnperf:/dest"] -D100k -P50k -R1b --monitor [monitor addr]	#-D100K is the total size of the data
												#-P50 is the size of a single payload sent
												#-P1b speed of bundle transmission (here 1 bundle per second)

check active dtnme demon
	ps -ax | grep dtnme
	
```