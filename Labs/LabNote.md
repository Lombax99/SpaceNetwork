
```
config file:
	/etc/dtnme_deamon.cfg
	#possono esserci domande nelle crocette riguardo questo file
	#non si vede solo con "ls" (usare "ls -la")
	
ssh connection.
	ips: student@10.0.0.11 [12, 13, 14]		pass: foobar
	
run dtme:
	cd ion
	sudo dtnme -d -o ./dtn.log -t		#-t clear the database data, fresh start
	
connect to dtnme demon (if it's running):
	telnet localhost 5050
	
command while in the demon:
	link dump										#fa vedere le connessioni
	route dump										#fa vedere le route
	link add [nomelink eg:ltvm1] [ip eg:10.0.0.1] [contatto eg:ALWAYSON] [tipo eg:tcp]	#crea un nuovo link
	link close [nomelink eg:ltvm1]								#chiude un link esistente
	link open [nomelink eg:ltvm1]								#apre un link chiuso
	
dtnperf tool:
	cd ion
	sudo dtnme -d -o ./dtn.log -t					                #attiva il demone
	dtnperf_vDTN_vBLABLA... --monitor				                #attiva il nodo come monitor
	dtnperf_vDTN_vBLABLA... --server [--force-eid IPN]				#attiva il nodo come server in ascolto [opz per cambiare il default e usare cgr, il server deve usare IPM]
	dtnperf_vDTN_vBLABLA... --client -d [destination node escluso "/dtnperf:/dest"] -D100k -P50k -R1b --monitor [monitor addr] 
													[bisogna indicare l'address del server tipo ipn:10.3 dinamicamente definito nel server al lancio per usare cgr]
													#-D100K is the total size of the data
													#-P50 is the size of a single payload sent
													#-R1b speed of bundle transmission (here 1 bundle per second)

check active dtnme demon
	ps -ax | grep dtnme
	
--- Lab 14/11/2023 ---
- obittivi: voglio usare l'algoritmo CGR piuttosto che routing statìco e essere in grado di forzare per ogni nodo l'eid che desidero
- sono state usate 3 macchine (vm1, vm2 e vm4) e tutti i channel emulator
	sudo nano /etc/dtnme_daemon.cfg									# mostra la configurazione del demone --> siamo andati a decommentare "route set type uniboCGR" e commentare riga precedente
Avvio il demone e mi interfaccio ad esso su ogni macchina:
	sudo dtnme -d -o ./dtn.log -t
	telnet localhost 5050
Avvio il server su vm2 (o vm4) forzando l'eid ad essere di tipo IPN:
	dtnperf_vDTN_vBLABLA... --server --force-eid IPN --ipn-local 4					# --ipn-local 2 è l'eid che abbiamo deciso noi
Avvio il client su vm1 specificando il nuovo eid del server a cui connettersi:
	dtnperf_vDTN_vBLABLA... --client -d ipn:2.2000 -D100k -P50k -R1b
Posso decidere di forzare un nuovo eid anche per il client invocandolo con:
	dtnperf_vDTN_vBLABLA... --client -d ipn:2.2000 -D100k -P50k -R1b --force-eid IPN --ipn-local 1
```

Link al documento dell'IEEE: https://ieeexplore.ieee.org/document/4530739

ToDo prep esame lab
- [ ] Testare la parte di Unibo-cgr
- [ ] Wireshark analisi file
- [ ] giocare un po' con i parametri per i bundle (link blocked)
- [ ] riprovare un link diretto vm1 to vm3

