.. include:: replace.txt
.. highlight:: cpp

..
	========================================================================================
	Current translation in Greek was done as a project of a ns3-related seminar organized by
	Greek Free/Open Source Software Society (ellak.gr) at University of Patras in 2014. The
	members of the translation team were:
		* Costas Deltouzos (costas.deltouzos@gmail.com)
		* Vasileios Dimitropoulos (vasdimitrop@upatras.gr)
		* Giorgos Kaffezas (kaffezas@ceid.upatras.gr)
		
	The team that is responsible for keeping the translation up-to-date consists of:
		# Vasileios Dimitropoulos (vasdimitrop@upatras.gr)
		# Giorgos Kaffezas (kaffezas@ceid.upatras.gr)
		# Nikos Stathopoulos (stathopou@ceid.upatras.gr)
		# Enea Tsanai (tsanai@ceid.upatras.gr)
	----------------------------------------------------------------------------------------
	> Current file was initially translated by Giorgos Kaffezas.
	> Last update was performed at 2015-04-28 by Giorgos Kaffezas.
	========================================================================================


.. Building Topologies

.. _BuildingTopologies:

Δημιουργία Τοπολογιών
---------------------

.. Building a Bus Network Topology

Δημιουργώντας μια Τοπολογία Δικτύου Αρτηρίας
********************************************

..
	In this section we are going to expand our mastery of |ns3| network 
	devices and channels to cover an example of a bus network.  |ns3|
	provides a net device and channel we call CSMA (Carrier Sense Multiple Access).
	
Σε αυτή την ενότητα πρόκειται να επεκτείνουμε την ικανότητά μας να διαχειριζόμαστε
τις συσκευές δικτύου του |ns3| και τα κανάλια, ώστε να καλύψουμε ένα παράδειγμα ενός δικτύου
αρτηρίας. O |ns3| παρέχει μια δικτυακή συσκευή και ένα κανάλι που εμείς αποκαλούμε CSMA
(Carrier Sense Multiple Access). 

..
	The |ns3| CSMA device models a simple network in the spirit of 
	Ethernet.  A real Ethernet uses CSMA/CD (Carrier Sense Multiple Access with 
	Collision Detection) scheme with exponentially increasing backoff to contend 
	for the shared transmission medium.  The |ns3| CSMA device and 
	channel models only a subset of this.
	
Η CSMA συσκευή του |ns3| μοντελοποιεί ένα απλό δίκτυο στο πνεύμα του Ethernet.
Ένα πραγματικό Ethernet χρησιμοποιεί το CSMA/CD (Carrier Sense Multiple Access with 
Collision Detection) σχήμα με εκθετικά αυξανόμενη οπισθοχώρηση για τη διεκδίκηση του
διαμοιραζόμενου μέσου μετάδοσης. Η CSMA συσκευή του |ns3| και το αντίστοιχο κανάλι 
μοντελοποιούν μόνο ένα υποσύνολο από αυτά. 

..
	Just as we have seen point-to-point topology helper objects when constructing
	point-to-point topologies, we will see equivalent CSMA topology helpers in
	this section.  The appearance and operation of these helpers should look 
	quite familiar to you.
	
Όπως έχουμε δει αντικείμενα βοηθών για τοπολογίες σημείου-προς-σημείο όταν κατασκευάζαμε
τέτοιες τοπολογίες, σε αυτή την ενότητα θα δούμε ισοδύναμους CSMA βοηθούς τοπολογίας. Η 
εμφάνιση και η λειτουργία αυτών των βοηθών θα πρέπει να σας φαίνεται αρκετά οικεία.

..
	We provide an example script in our examples/tutorial} directory.  This script
	builds on the ``first.cc`` script and adds a CSMA network to the 
	point-to-point simulation we've already considered.  Go ahead and open 
	``examples/tutorial/second.cc`` in your favorite editor.  You will have already seen
	enough |ns3| code to understand most of what is going on in this 
	example, but we will go over the entire script and examine some of the output.
	
Παραθέτουμε ένα παραδειγματικό σενάριο στον κατάλογο examples/tutorial. Το σενάριο αυτό
«πατάει» πάνω στο σενάριο ``first.cc`` και προσθέτει ένα δίκτυο CSMA στην προσομοίωση
σημείου-προς-σημείο που έχουμε ήδη εξετάσει. Ανοίξτε το ``examples/tutorial/second.cc``
στον επεξεργαστή κειμένου της προτίμησής σας. Θα πρέπει να έχετε ήδη δει αρκετό κώδικα του 
|ns3| ώστε να μπορείτε να καταλάβετε τα περισσότερα από όσα συμβαίνουν σε αυτό το παράδειγμα, αλλά
εμείς θα δούμε ολόκληρο το σενάριο και θα εξετάσουμε κάποια από τα αποτελέσματα.

..
	Just as in the ``first.cc`` example (and in all ns-3 examples) the file
	begins with an emacs mode line and some GPL boilerplate.
	
Όπως και στο παράδειγμα του ``first.cc`` (και σε όλα τα παραδείγματα του ns-3), το αρχείο
ξεκινάει με μια γραμμή κατάστασης για τον emacs και κάποιες κοινές δηλώσεις GPL.

..
	The actual code begins by loading module include files just as was done in the
	``first.cc`` example.
	
Ο πραγματικός κώδικας ξεκινάει με τη φόρτωση αρχείων συμπερίληψης ενοτήτων (module includes), όπως έγινε
και στο παράδειγμα ``first.cc``.

::

  #include "ns3/core-module.h"
  #include "ns3/network-module.h"
  #include "ns3/csma-module.h"
  #include "ns3/internet-module.h"
  #include "ns3/point-to-point-module.h"
  #include "ns3/applications-module.h"
  #include "ns3/ipv4-global-routing-helper.h"

..
	One thing that can be surprisingly useful is a small bit of ASCII art that
	shows a cartoon of the network topology constructed in the example.  You will
	find a similar "drawing" in most of our examples.
	
Ένα πράγμα που μπορεί να είναι εκπληκτικά χρήσιμο είναι ένα μικρό κομμάτι τέχνης σε
ASCII, το οποίο δείχνει ένα «σχέδιο» της τοπολογίας δικτύου που κατασκευάζεται στο
παράδειγμα. Θα βρείτε ένα παρόμοιο «σχέδιο» στα περισσότερα από τα παραδείγματά μας.

..
	In this case, you can see that we are going to extend our point-to-point
	example (the link between the nodes n0 and n1 below) by hanging a bus network
	off of the right side.  Notice that this is the default network topology 
	since you can actually vary the number of nodes created on the LAN.  If you
	set nCsma to one, there will be a total of two nodes on the LAN (CSMA 
	channel) --- one required node and one "extra" node.  By default there are
	three "extra" nodes as seen below:
	
Σε αυτήν την περίπτωση, μπορείτε να δείτε ότι πρόκειται να επεκτείνουμε το παράδειγμά
μας από σημείο-προς-σημείο (ο σύνδεσμος μεταξύ των κόμβων n0 και n1 παρακάτω) συνδέοντας 
ένα δίκτυο αρτηρίας στη δεξιά του πλευρά. Παρατηρήστε ότι αυτή είναι η προεπιλεγμένη
τοπολογία δικτύου καθώς μπορείτε ουσιαστικά να αλλάξετε τον αριθμό των κόμβων που 
δημιουργούνται στο LAN. Εάν θέσετε τη nCsma στην τιμή ένα, θα υπάρχουν συνολικά δύο 
κόμβοι στο LAN (CSMA κανάλι) --- ένας απαιτούμενος κόμβος και ένας «επιπλέον» κόμβος. Εξ
ορισμού υπάρχουν τρεις «επιπλέον» κόμβοι, όπως φαίνεται παρακάτω:

::

// Default Network Topology
//
//       10.1.1.0
// n0 -------------- n1   n2   n3   n4
//    point-to-point  |    |    |    |
//                    ================
//                      LAN 10.1.2.0

..
	Then the ns-3 namespace is ``used`` and a logging component is defined.
	This is all just as it was in ``first.cc``, so there is nothing new yet.
	
Έπειτα χρησιμοποιείται (``used``) ο χώρος ονομάτων του ns-3 και ορίζεται ένα στοιχείο
καταγραφής. Όλα αυτά είναι όπως ήταν και στο ``first.cc``, οπότε δεν υπάρχει κάτι
καινούργιο ακόμα.

::
  
  using namespace ns3;
  
  NS_LOG_COMPONENT_DEFINE ("SecondScriptExample");

..
	The main program begins with a slightly different twist.  We use a verbose
	flag to determine whether or not the ``UdpEchoClientApplication`` and
	``UdpEchoServerApplication`` logging components are enabled.  This flag
	defaults to true (the logging components are enabled) but allows us to turn
	off logging during regression testing of this example.
	
Το κυρίως πρόγραμμα ξεκινά με λίγο διαφορετική εξέλιξη. Χρησιμοποιούμε μια σημαία
verbose για να καθορίσουμε εάν θα ενεργοποιηθούν τα στοιχεία καταγραφής των
``UdpEchoClientApplication`` και ``UdpEchoServerApplication``. Αυτή η σημαία 
τίθεται εξ ορισμού ως αληθής (τα στοιχεία καταγραφής είναι ενεργοποιημένα) αλλά μας
επιτρέπει να απενεργοποιήσουμε την καταγραφή κατά τη διάρκεια της προς-τα-πίσω δοκιμής
αυτού του παραδείγματος.

..
	You will see some familiar code that will allow you to change the number
	of devices on the CSMA network via command line argument.  We did something
	similar when we allowed the number of packets sent to be changed in the section
	on command line arguments.  The last line makes sure you have at least one
	"extra" node.
	
Θα δείτε μερικό οικείο κώδικα, ο οποίος θα σας επιτρέψει να αλλάξετε τον αριθμό
των συσκευών στο CSMA δίκτυο μέσω ορισμάτων στη γραμμή εντολών. Κάναμε κάτι παρόμοιο
όταν επιτρέψαμε να αλλάξει ο αριθμός των πακέτων που στέλνονται στην ενότητα με τα
ορίσματα της γραμμής εντολών. Η τελευταία γραμμή διασφαλίζει ότι έχετε τουλάχιστον
έναν «επιπλέον» κόμβο.

..
	The code consists of variations of previously covered API so you should be
	entirely comfortable with the following code at this point in the tutorial.
	
Ο κώδικας αποτελείται από παραλλαγές του API που έχουμε εξετάσει πιο πριν, οπότε θα 
πρέπει να είστε πλήρως άνετοι με τον ακόλουθο κώδικα σε αυτό το σημείο του οδηγού.

::

  bool verbose = true;
  uint32_t nCsma = 3;

  CommandLine cmd;
  cmd.AddValue ("nCsma", "Number of \"extra\" CSMA nodes/devices", nCsma);
  cmd.AddValue ("verbose", "Tell echo applications to log if true", verbose);

  cmd.Parse (argc, argv);

  if (verbose)
    {
      LogComponentEnable("UdpEchoClientApplication", LOG_LEVEL_INFO);
      LogComponentEnable("UdpEchoServerApplication", LOG_LEVEL_INFO);
    }

  nCsma = nCsma == 0 ? 1 : nCsma;

..
	The next step is to create two nodes that we will connect via the 
	point-to-point link.  The ``NodeContainer`` is used to do this just as was
	done in ``first.cc``.
	
Το επόμενο βήμα είναι η δημιουργία δύο κόμβων, τους οποίους θα συνδέσουμε μέσω
ενός συνδέσμου σημείου-προς-σημείο. Χρησιμοποιείται ο ``NodeContainer`` για να 
το κάνει αυτό, ακριβώς όπως το έκανε και στο ``first.cc``.

::

  NodeContainer p2pNodes;
  p2pNodes.Create (2);

..
	Next, we declare another ``NodeContainer`` to hold the nodes that will be
	part of the bus (CSMA) network.  First, we just instantiate the container
	object itself.  
	
Έπειτα, δηλώνουμε άλλον ένα ``NodeContainer``, ο οποίος θα περιέχει τους κόμβους
που θα είναι μέρος του δικτύου αρτηρίας (CSMA). Αρχικά, πρέπει να δημιουργήσουμε
το αντικείμενο container αυτό καθαυτό.

::

  NodeContainer csmaNodes;
  csmaNodes.Add (p2pNodes.Get (1));
  csmaNodes.Create (nCsma);

..
	The next line of code ``Gets`` the first node (as in having an index of one)
	from the point-to-point node container and adds it to the container of nodes
	that will get CSMA devices.  The node in question is going to end up with a 
	point-to-point device *and* a CSMA device.  We then create a number of 
	"extra" nodes that compose the remainder of the CSMA network.  Since we 
	already have one node in the CSMA network -- the one that will have both a
	point-to-point and CSMA net device, the number of "extra" nodes means the
	number nodes you desire in the CSMA section minus one.
	
Η επόμενη γραμμή κώδικα παίρνει (``Gets``) τον πρώτο κόμβο (δηλαδή σα να έχει ένα ευρετήριο
που να περιέχει έναν κόμβο) από τον container κόμβων σημείου-προς-σημείο και τον προσθέτει
στον container των κόμβων που θα δεχτούν τις CSMA συσκευές. Ο εν λόγω κόμβος πρόκειται
να καταλήξει με μια συσκευή σημείου-προς-σημείο *και* μια CSMA συσκευή. Έπειτα δημιουργούμε
έναν αριθμό από «επιπλέον» κόμβους που συνθέτουν το υπόλοιπο του CSMA δικτύου. Δεδομένου ότι
έχουμε ήδη έναν κόμβο στο CSMA δίκτυο -- εκείνον που θα έχει και μια δικτυακή συσκευή
σημείου-προς-σημείο και μια CSMA, ο αριθμός των «επιπλέον» κόμβων ισούται με τον αριθμό
των κόμβων που επιθυμείτε στο κομμάτι του CSMA πλην ενός.

..
	The next bit of code should be quite familiar by now.  We instantiate a
	``PointToPointHelper`` and set the associated default ``Attributes`` so
	that we create a five megabit per second transmitter on devices created using
	the helper and a two millisecond delay on channels created by the helper.
	
Το επόμενο κομμάτι κώδικα θα πρέπει να σας είναι ήδη πολύ οικείο. Δημιουργούμε έναν
``PointToPointHelper`` και καθορίζουμε τα σχετικά προεπιλεγμένα ``Attributes``, έτσι ώστε
να δημιουργήσουμε έναν πομπό με ταχύτητα μετάδοσης πέντε megabit ανά δευτερόλεπτο στις 
συσκευές που δημιουργήθηκαν με τη χρήση του βοηθού και μια καθυστέρηση δύο μιλιδευτερολέπτων
στα κανάλια που δημιουργήθηκαν από τον βοηθό. 

::

  PointToPointHelper pointToPoint;
  pointToPoint.SetDeviceAttribute ("DataRate", StringValue ("5Mbps"));
  pointToPoint.SetChannelAttribute ("Delay", StringValue ("2ms"));

  NetDeviceContainer p2pDevices;
  p2pDevices = pointToPoint.Install (p2pNodes);

..
	We then instantiate a ``NetDeviceContainer`` to keep track of the 
	point-to-point net devices and we ``Install`` devices on the 
	point-to-point nodes.
	
Έπειτα δημιουργούμε ένα ``NetDeviceContainer`` για να καταγράφει τις δικτυακές
συσκευές σημείου-προς-σημείο και εγκαθιστούμε (``Install``) τις συσκευές στους κόμβους
σημείου-προς-σημείο.

..
	We mentioned above that you were going to see a helper for CSMA devices and
	channels, and the next lines introduce them.  The ``CsmaHelper`` works just
	like a ``PointToPointHelper``, but it creates and connects CSMA devices and
	channels.  In the case of a CSMA device and channel pair, notice that the data
	rate is specified by a *channel* ``Attribute`` instead of a device 
	``Attribute``.  This is because a real CSMA network does not allow one to mix,
	for example, 10Base-T and 100Base-T devices on a given channel.  We first set 
	the data rate to 100 megabits per second, and then set the speed-of-light delay
	of the channel to 6560 nano-seconds (arbitrarily chosen as 1 nanosecond per foot
	over a 100 meter segment).  Notice that you can set an ``Attribute`` using 
	its native data type.
	
Αναφέραμε προηγουμένως πως πρόκειται να δείτε έναν βοηθό για CSMA συσκευές και 
κανάλια, και οι επόμενες γραμμές εισάγουν αυτά τα στοιχεία. Ο ``CsmaHelper`` λειτουργεί
όπως ένας ``PointToPointHelper``, μόνο που δημιουργεί και συνδέει CSMA συσκευές και 
κανάλια. Στην περίπτωση ενός ζεύγους CSMA συσκευής και καναλιού, παρατηρήστε ότι ο
ρυθμός δεδομένων καθορίζεται μέσω ενός ``Attribute`` του *καναλιού* αντί για κάποιο
``Attribute`` της συσκευής. Αυτό συμβαίνει επειδή ένα πραγματικό CSMA δίκτυο δεν επιτρέπει
την ανάμειξη, για παράδειγμα, συσκευών 10Base-T και 100Base-T σε ένα δεδομένο κανάλι. Πρώτα
θέτουμε το ρυθμό δεδομένων στα 100 megabit ανά δευτερόλεπτο, και μετά θέτουμε την καθυστέρηση
του καναλιού στα 6560 νανοδευτερόλεπτα (έχοντας επιλέξει αυθαίρετα ότι χρειάζεται 1
νανοδευτερόλεπτο ανά πόδι αντί ανά τμήματα 100 μέτρων). Σημειώστε ότι μπορείτε να καθορίσετε
ένα ``Attribute`` χρησιμοποιώντας τον ενδογενή τύπο δεδομένων του.

::

  CsmaHelper csma;
  csma.SetChannelAttribute ("DataRate", StringValue ("100Mbps"));
  csma.SetChannelAttribute ("Delay", TimeValue (NanoSeconds (6560)));

  NetDeviceContainer csmaDevices;
  csmaDevices = csma.Install (csmaNodes);

..
	Just as we created a ``NetDeviceContainer`` to hold the devices created by
	the ``PointToPointHelper`` we create a ``NetDeviceContainer`` to hold 
	the devices created by our ``CsmaHelper``.  We call the ``Install`` 
	method of the ``CsmaHelper`` to install the devices into the nodes of the
	``csmaNodes NodeContainer``.
	
Με τον ίδιο τρόπο που δημιουργήσαμε ένα ``NetDeviceContainer`` ώστε να περιέχει τις
συσκευές που δημιουργούνται από τον ``PointToPointHelper``, έτσι δημιουργούμε ένα 
``NetDeviceContainer`` για να περιέχει τις συσκευές που δημιουργούνται από τον ``CsmaHelper``
μας. Καλούμε τη μέθοδο ``Install`` του ``CsmaHelper`` ώστε να εγκαταστήσουμε τις συσκευές
στους κόμβους του ``csmaNodes NodeContainer``.

..
	We now have our nodes, devices and channels created, but we have no protocol
	stacks present.  Just as in the ``first.cc`` script, we will use the
	``InternetStackHelper`` to install these stacks.
	
Τώρα έχουμε δημιουργήσει τους κόμβους μας, τις συσκευές και τα κανάλια μας, αλλά δεν έχουμε 
καμία εγκατεστημένη στοίβα πρωτοκόλλου. Όπως και στο σενάριο του ``first.cc``, θα 
χρησιμοποιήσουμε τον ``InternetStackHelper`` για να εγκαταστησουμε αυτές τις στοίβες.

::

  InternetStackHelper stack;
  stack.Install (p2pNodes.Get (0));
  stack.Install (csmaNodes);

..
	Recall that we took one of the nodes from the ``p2pNodes`` container and
	added it to the ``csmaNodes`` container.  Thus we only need to install 
	the stacks on the remaining ``p2pNodes`` node, and all of the nodes in the
	``csmaNodes`` container to cover all of the nodes in the simulation.

Θυμηθείτε ότι πήραμε έναν από τους κόμβους από τον container ``p2pNodes`` και τον
προσθέσαμε στον container ``csmaNodes``. Έτσι χρειάζεται μόνο να εγκαταστήσουμε 
τις στοίβες στον εναπομείναντα κόμβου του ``p2pNodes``, και σε όλους τους κόμβους
του container ``csmaNodes``, ώστε να καλύψουμε όλους τους κόμβους της προσομοίωσης.

..
	Just as in the ``first.cc`` example script, we are going to use the 
	``Ipv4AddressHelper`` to assign IP addresses to our device interfaces.
	First we use the network 10.1.1.0 to create the two addresses needed for our
	two point-to-point devices.
	
Όπως και στο παράδειγμα του ``first.cc``, πρόκειται να χρησιμοποιήσουμε τον 
``Ipv4AddressHelper`` για την ανάθεση των IP διευθύνσεων στις διεπαφές των συσκευών
μας. Αρχικά χρησιμοποιούμε το δίκτυο 10.1.1.0 για να δημιουργήσουμε τις δύο
διευθύνσεις που χρειάζονται για τις δύο σημείο-προς-σημείο συσκευές μας.

::

  Ipv4AddressHelper address;
  address.SetBase ("10.1.1.0", "255.255.255.0");
  Ipv4InterfaceContainer p2pInterfaces;
  p2pInterfaces = address.Assign (p2pDevices);

..
	Recall that we save the created interfaces in a container to make it easy to
	pull out addressing information later for use in setting up the applications.
	
Θυμηθείτε ότι αποθηκεύουμε τις δημιουργημένες διεπαφές σε ένα container ώστε να κάνουμε
πιο εύκολη την ανάκτηση πληροφορίας διευθυνσιοδότησης αργότερα, για χρήση κατά το στήσιμο
των εφαρμογών.

..
	We now need to assign IP addresses to our CSMA device interfaces.  The 
	operation works just as it did for the point-to-point case, except we now
	are performing the operation on a container that has a variable number of 
	CSMA devices --- remember we made the number of CSMA devices changeable by 
	command line argument.  The CSMA devices will be associated with IP addresses 
	from network number 10.1.2.0 in this case, as seen below.
	
Τώρα πρέπει να αναθέσουμε IP διευθύνσεις στις διεπαφές των CSMA συσκευών μας. Η 
λειτουργία πραγματοποιείται όπως και στην περίπτωση σημείου-προς-σημείο, με τη διαφορά
ότι τώρα πραγματοποιούμε τη λειτουργία σε ένα container που έχει ένα μεταβλητό αριθμό
από CSMA συσκευές --- θυμηθείτε ότι κάναμε τον αριθμό των CSMA συσκευών μεταβλητό μέσω
ορίσματος στη γραμμή εντολών. Οι CSMA συσκευές θα συσχετιστούν σε αυτήν την περίπτωση
με τις IP διευθύνσεις από τη διεύθυνση δικτύου 10.1.2.0, όπως βλέπετε παρακάτω.

::

  address.SetBase ("10.1.2.0", "255.255.255.0");
  Ipv4InterfaceContainer csmaInterfaces;
  csmaInterfaces = address.Assign (csmaDevices);

..
	Now we have a topology built, but we need applications.  This section is
	going to be fundamentally similar to the applications section of 
	``first.cc`` but we are going to instantiate the server on one of the 
	nodes that has a CSMA device and the client on the node having only a 
	point-to-point device.

Τώρα έχουμε δημιουργήσει μια τοπολογία, αλλά χρειαζόμαστε εφαρμογές. Αυτή η ενότητα
θα είναι κατά βάση παρόμοια με το τμήμα των εφαρμογών στο ``first.cc``, αλλά εδώ θα 
εκκινήσουμε τον εξυπηρετητή σε έναν από τους κόμβους ο οποίος έχει μια CSMA συσκευή ,
και τον πελάτη στον κόμβο που έχει μόνο μία συσκευή σημείου-προς-σημείο.

..
	First, we set up the echo server.  We create a ``UdpEchoServerHelper`` and
	provide a required ``Attribute`` value to the constructor which is the server
	port number.  Recall that this port can be changed later using the 
	``SetAttribute`` method if desired, but we require it to be provided to
	the constructor.

Αρχικά, στήνουμε έναν εξυπηρετητή echo. Δημιουργούμε έναν ``UdpEchoServerHelper`` και
παρέχουμε μια απαιτούμενη τιμή για ``Attribute`` στον δημιουργό, η οποία είναι ο αριθμός
port του εξυπηρετητή. Θυμηθείτε ότι αυτό το port μπορεί να αλλάξει αργότερα, εάν το επιθυμείτε, 
με τη χρήση της μεθόδου ``SetAttribute``, αλλά εμείς απαιτούμε να δίνεται ως όρισμα στον δημιουργό.

::

  UdpEchoServerHelper echoServer (9);

  ApplicationContainer serverApps = echoServer.Install (csmaNodes.Get (nCsma));
  serverApps.Start (Seconds (1.0));
  serverApps.Stop (Seconds (10.0));

..
	Recall that the ``csmaNodes NodeContainer`` contains one of the 
	nodes created for the point-to-point network and ``nCsma`` "extra" nodes. 
	What we want to get at is the last of the "extra" nodes.  The zeroth entry of
	the ``csmaNodes`` container will be the point-to-point node.  The easy
	way to think of this, then, is if we create one "extra" CSMA node, then it
	will be at index one of the ``csmaNodes`` container.  By induction,
	if we create ``nCsma`` "extra" nodes the last one will be at index 
	``nCsma``.  You see this exhibited in the ``Get`` of the first line of 
	code.
	
Θυμηθείτε ότι ο ``csmaNodes NodeContainer`` περιέχει έναν από τους κόμβους που 
δημιουργήθηκαν στο δίκτυο σημείου-προς-σημείο και ``nCsma`` «επιπλέον» κόμβους.
Εκεί που θέλουμε να φτάσουμε είναι στον τελευταίο από τους "επιπλέον" κόμβους. Οπότε,
ο εύκολος τρόπος για να το σκεφτούμε είναι πως εάν δημιουργήσουμε έναν «επιπλέον» CSMA κόμβο,
τότε αυτός θα βρίσκεται στην πρώτη θέση του container ``csmaNodes``. Επαγωγικά, εάν 
δημιουργήσουμε ``nCsma`` «επιπλέον» κόμβους, ο τελευταίος θα είναι στη θέση ``nCsma``.
Μπορείτε να το δείτε αυτό στο ``Get`` της πρώτης γραμμής του κώδικα.

..
	The client application is set up exactly as we did in the ``first.cc``
	example script.  Again, we provide required ``Attributes`` to the 
	``UdpEchoClientHelper`` in the constructor (in this case the remote address
	and port).  We tell the client to send packets to the server we just installed
	on the last of the "extra" CSMA nodes.  We install the client on the 
	leftmost point-to-point node seen in the topology illustration.
	
Η εφαρμογή του πελάτη εγκαθίσταται ακριβώς όπως και στο σενάριο του ``first.cc``. Ξανά,
παρέχουμε τα απαιτούμενα ``Attributes`` στον ``UdpEchoClientHelper`` μέσα στον δημιουργό
(σε αυτή την περίπτωση την απομακρυσμένη διεύθυνση και το port). Λέμε στον πελάτη να
στέλνει πακέτα στον εξυπηρετητή που μόλις έχουμε εγκαταστήσει στον τελευταίο από τους
«επιπλέον» CSMA κόμβους. Εγκαθιστούμε τον πελάτη στον αριστερότερο κόμβο του σημείου-προς-σημείο
κομματιού που φαίνεται στην απεικόνιση της τοπολογίας.

::

  UdpEchoClientHelper echoClient (csmaInterfaces.GetAddress (nCsma), 9);
  echoClient.SetAttribute ("MaxPackets", UintegerValue (1));
  echoClient.SetAttribute ("Interval", TimeValue (Seconds (1.0)));
  echoClient.SetAttribute ("PacketSize", UintegerValue (1024));

  ApplicationContainer clientApps = echoClient.Install (p2pNodes.Get (0));
  clientApps.Start (Seconds (2.0));
  clientApps.Stop (Seconds (10.0));

..
	Since we have actually built an internetwork here, we need some form of 
	internetwork routing.  |ns3| provides what we call global routing to
	help you out.  Global routing takes advantage of the fact that the entire 
	internetwork is accessible in the simulation and runs through the all of the
	nodes created for the simulation --- it does the hard work of setting up routing 
	for you without having to configure routers.
	
Καθώς έχουμε στήσει στην ουσία ένα διαδίκτυο εδώ, χρειαζόμαστε κάποια μορφή διαδικτυακής
δρομολόγησης. Ο |ns3| παρέχει αυτό που εμείς αποκαλούμε «καθολική δρομολόγηση» (global routing), προκειμένου
να σας βοηθήσει. Η καθολική δρομολόγηση εκμεταλλεύεται το γεγονός ότι ολόκληρο το διαδίκτυο
είναι προσβάσιμο μέσα στην προσομοίωση και εκτελείται διαμέσου όλων των κόμβων που
έχουν δημιουργηθεί για την προσομοίωση --- αναλαμβάνει την δύσκολη δουλειά του καθορισμού της
δρομολόγησης για εσάς χωρίς να χρειάζεται να ρυθμίσετε εσείς δρομολογητές.

..
	Basically, what happens is that each node behaves as if it were an OSPF router
	that communicates instantly and magically with all other routers behind the
	scenes.  Each node generates link advertisements and communicates them 
	directly to a global route manager which uses this global information to 
	construct the routing tables for each node.  Setting up this form of routing
	is a one-liner:
	
Βασικά, αυτό που συμβαίνει είναι ότι κάθε κόμβος συμπεριφέρεται σα να ήταν ένας δρομολογητής
OSPF, ο οποίος επικοινωνεί άμεσα και μαγικά με όλους τους άλλους δρομολογητές στο παρασκήνιο.
Κάθε κόμβος παράγει δηλώσεις των συνδέσεων και τις μεταδίδει κατευθείαν σε έναν διαχειριστή 
καθολικής δρομολόγησης, ο οποίος χρησιμοποιεί αυτή την καθολική πληροφορία για να κατασκευάσει
τους πίνακες δρομολόγησης σε κάθε κόμβο. Η εγκατάσταση μιας τέτοιας μορφής δρομολόγησης γίνεται
σε μία γραμμή:

::

  Ipv4GlobalRoutingHelper::PopulateRoutingTables ();

..
	Next we enable pcap tracing.  The first line of code to enable pcap tracing 
	in the point-to-point helper should be familiar to you by now.  The second
	line enables pcap tracing in the CSMA helper and there is an extra parameter
	you haven't encountered yet.
	
Έπειτα ενεργοποιούμε την καταγραφή pcap. Η πρώτη γραμμή του κώδικα που ενεργοποιεί την
καταγραφή pcap στον βοηθό σημείου-προς-σημείο θα πρέπει να σας είναι οικεία μέχρι τώρα.
Η δεύτερη γραμμή ενεργοποιεί την καταγραφή pcap στον CSMA βοηθό και υπάρχει μία επιπλέον
παράμετρος που δεν έχετε συναντήσει ακόμα.

::

  pointToPoint.EnablePcapAll ("second");
  csma.EnablePcap ("second", csmaDevices.Get (1), true);

..
	The CSMA network is a multi-point-to-point network.  This means that there 
	can (and are in this case) multiple endpoints on a shared medium.  Each of 
	these endpoints has a net device associated with it.  There are two basic
	alternatives to gathering trace information from such a network.  One way 
	is to create a trace file for each net device and store only the packets
	that are emitted or consumed by that net device.  Another way is to pick 
	one of the devices and place it in promiscuous mode.  That single device
	then "sniffs" the network for all packets and stores them in a single
	pcap file.  This is how ``tcpdump``, for example, works.  That final 
	parameter tells the CSMA helper whether or not to arrange to capture 
	packets in promiscuous mode.
	
Το CSMA δίκτυο είναι ένα πολλαπλό σημείο-προς-σημείο δίκτυο. Αυτό σημαίνει ότι
μπορούν να υπάρχουν (και όντως υπάρχουν σε αυτήν την περίπτωση) πολλαπλά τερματικά σημεία
σε ένα κοινόχρηστο μέσο. Κάθε ένα από αυτά τα τερματικά σημεία έχει μια δικτυακή συσκευή
που συσχετίζεται με αυτό. Υπάρχουν δύο βασικές εναλλακτικές για τη συλλογή πληροφορίας ιχνών
από ένα τέτοιο δίκτυο. Η μία είναι η δημιουργία ενός αρχείου καταγραφής για κάθε μία δικτυακή
συσκευή και η αποθήκευση μόνο των πακέτων που μεταδίδονται ή καταναλώνονται από αυτήν την
δικτυακή συσκευή. Μια άλλη εναλλακτική είναι η επιλογή μίας από τις συσκευές και η μετάβασή της
σε μεικτή κατάσταση. Έτσι, αυτή μόνο η συσκευή «παρακολουθεί» (sniff) το δίκτυο για όλα τα πακέτα
και τα αποθηκεύει σε ένα μοναδικό αρχείο pcap. Με αυτόν τον τρόπο λειτουργεί, για παράδειγμα, το
``tcpdump``. Εκείνη η τελική παράμετρος λέει στον CSMA βοηθό εάν πρέπει ή όχι να κανονίσει ώστε να
δεσμεύει πακέτα σε μεικτή κατάσταση.

..
	In this example, we are going to select one of the devices on the CSMA
	network and ask it to perform a promiscuous sniff of the network, thereby
	emulating what ``tcpdump`` would do.  If you were on a Linux machine
	you might do something like ``tcpdump -i eth0`` to get the trace.  
	In this case, we specify the device using ``csmaDevices.Get(1)``, 
	which selects the first device in the container.  Setting the final
	parameter to true enables promiscuous captures.
	
Σε αυτό το παράδειγμα, θα επιλέξουμε μία από τις συσκευές στο CSMA δίκτυο και θα της
ζητήσουμε να εκτελέσει μία μεικτή παρακολούθηση (promiscuous sniff) του δικτύου, 
προσομοιώνοντας έτσι αυτό που θα έκανε το ``tcpdump``. Εάν ήσασταν σε ένα μηχάνημα με Linux,
θα μπορούσατε να δώσετε την εντολή ``tcpdump -i eth0`` προκειμένου να πάρετε τα ίχνη.
Σε αυτήν την περίπτωση, προσδιορίζουμε τη συσκευή χρησιμοποιώντας την ``csmaDevices.Get(1)``, 
η οποία επιλέγει την πρώτη συσκευή στον container. Θέτοντας την τελευταία παράμετρο ως αληθή
ενεργοποιούνται οι μεικτές καταγραφές (promiscuous captures).

..
	The last section of code just runs and cleans up the simulation just like
	the ``first.cc`` example.
	
Το τελευταίο μέρος του κώδικα απλά τρέχει και καθαρίζει μετά την προσομοίωση, όπως 
και στο παράδειγμα του ``first.cc``.

::

    Simulator::Run ();
    Simulator::Destroy ();
    return 0;
  }

..
	In order to run this example, copy the ``second.cc`` example script into 
	the scratch directory and use waf to build just as you did with
	the ``first.cc`` example.  If you are in the top-level directory of the
	repository you just type,
	
Προκειμένου να εκτελέσετε αυτό το παράδειγμα, αντιγράψτε το παράδειγμα σεναρίου του 
``second.cc`` στον κατάλογο scratch και χρησιμοποιήστε το waf για το build όπως κάνατε
και στο παράδειγμα ``first.cc``. Εάν είστε στον υψηλότερου επιπέδου κατάλογο του αποθετηρίου,
απλά πληκτρολογήστε,

.. sourcecode:: bash

  $ cp examples/tutorial/second.cc scratch/mysecond.cc
  $ ./waf

..
	Warning:  We use the file ``second.cc`` as one of our regression tests to
	verify that it works exactly as we think it should in order to make your
	tutorial experience a positive one.  This means that an executable named 
	``second`` already exists in the project.  To avoid any confusion
	about what you are executing, please do the renaming to ``mysecond.cc``
	suggested above.
	
Προειδοποίηση: χρησιμοποιούμε το αρχείο ``second.cc`` ως ένα από τα τεστ οπισθοδρόμησής μας
προκειμένου να επικυρώσουμε ότι δουλεύει όπως ακριβώς πιστεύουμε ότι πρέπει να δουλεύει, ώστε
η εμπειρία σας με τον παρόντα οδηγό να είναι θετική. Αυτό σημαίνει ότι υπάρχει ήδη ένα
εκτελέσιμο αρχείο με το όνομα ``second`` σε αυτό το project. Για να αποφύγετε οποιαδήποτε
σύγχυση σχετικά με το τι εκτελείτε, παρακαλούμε κάντε τη μετονομασία σε ``mysecond.cc`` που 
προτείνεται παραπάνω.

..
	If you are following the tutorial religiously (you are, aren't you) you will
	still have the NS_LOG variable set, so go ahead and clear that variable and
	run the program.
	
Εάν ακολουθείτε αυτόν τον οδηγό με θρησκευτική ευλάβεια (το κάνετε, έτσι δεν είναι;) θα
έχετε ακόμα τη μεταβλητή NS_LOG τεθειμένη, οπότε καθαρίστε αυτή τη μεταβλητή και
εκτελέστε το πρόγραμμα.

.. sourcecode:: bash

  $ export NS_LOG=
  $ ./waf --run scratch/mysecond

..
	Since we have set up the UDP echo applications to log just as we did in 
	``first.cc``, you will see similar output when you run the script.

Δεδομένου ότι έχουμε στήσει τις εφαρμογές UDP echo για καταγραφή, ακριβώς όπως κάναμε
και στο ``first.cc``, θα δείτε μια παρόμοια έξοδο όταν τρέξετε το σενάριο.

.. sourcecode:: text

  Waf: Entering directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  Waf: Leaving directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  'build' finished successfully (0.415s)
  Sent 1024 bytes to 10.1.2.4
  Received 1024 bytes from 10.1.1.1
  Received 1024 bytes from 10.1.2.4

..
	Recall that the first message, "``Sent 1024 bytes to 10.1.2.4``," is the 
	UDP echo client sending a packet to the server.  In this case, the server
	is on a different network (10.1.2.0).  The second message, "``Received 1024 
	bytes from 10.1.1.1``," is from the UDP echo server, generated when it receives
	the echo packet.  The final message, "``Received 1024 bytes from 10.1.2.4``,"
	is from the echo client, indicating that it has received its echo back from
	the server.
	
Θυμηθείτε ότι το πρώτο μήνυμα, "``Sent 1024 bytes to 10.1.2.4``", είναι ο UDP echo πελάτης
που στέλνει ένα πακέτο στον εξυπηρετητή. Στην προκειμένη περίπτωση, ο εξυπηρετητής είναι
σε ένα διαφορετικό δίκτυο (10.1.2.0). Το δεύτερο μήνυμα, "``Received 1024 bytes from 10.1.1.1``", 
είναι από τον UDP echo εξυπηρετητή, και δημιουργήθηκε όταν αυτός έλαβε το echo πακέτο. Το τελικό
μήνυμα, "``Received 1024 bytes from 10.1.2.4``", είναι από τον echo πελάτη, και δείχνει ότι αυτός
έχει λάβει το echo μήνυμα από τον εξυπηρετητή.

..
	If you now go and look in the top level directory, you will find three trace 
	files:
	
Εάν τώρα πάτε και δείτε στον κατάλογο του υψηλότερου επιπέδου, θα βρείτε τρία αρχεία καταγραφής
ιχνών:

.. sourcecode:: text

  second-0-0.pcap  second-1-0.pcap  second-2-0.pcap

..
	Let's take a moment to look at the naming of these files.  They all have the 
	same form, ``<name>-<node>-<device>.pcap``.  For example, the first file
	in the listing is ``second-0-0.pcap`` which is the pcap trace from node 
	zero, device zero.  This is the point-to-point net device on node zero.  The 
	file ``second-1-0.pcap`` is the pcap trace for device zero on node one,
	also a point-to-point net device; and the file ``second-2-0.pcap`` is the
	pcap trace for device zero on node two.
	
Ας δούμε για μια στιγμή την ονομασία αυτών των αρχείων. Όλα έχουν την ίδια μορφή,
``<name>-<node>-<device>.pcap``. Για παράδειγμα, το πρώτο αρχείο στην παραπάνω απαρίθμηση
είναι το ``second-0-0.pcap``, το οποίο είναι το αρχείο ιχνών pcap από τον κόμβο μηδέν, από
τη συσκευή μηδέν. Αυή είναι η συσκευή σημείου-προς-σημείο στον κόμβο μηδέν. Το αρχείο
``second-1-0.pcap`` είναι το αρχείο ιχνών pcap για τη συσκευή μηδέν στον κόμβο ένα, η οποία
είναι επίσης μια δικτυακή συσκευή σημείου-προς-σημείο. Και το αρχείο ``second-2-0.pcap`` είναι
το αρχείο ιχνών pcap για τη συσκευή μηδέν στον κόμβο δύο.

..
	If you refer back to the topology illustration at the start of the section, 
	you will see that node zero is the leftmost node of the point-to-point link
	and node one is the node that has both a point-to-point device and a CSMA 
	device.  You will see that node two is the first "extra" node on the CSMA
	network and its device zero was selected as the device to capture the 
	promiscuous-mode trace.
	
Εάν ανατρέξετε πίσω στην απεικόνιση της τοπολογίας στην αρχή αυτής της ενότητας,
θα δείτε ότι ο κόμβος μηδέν είναι ο αριστερότερος κόμβος στη σύνδεση σημείου-προς-σημείο,
και ο κόμβος ένα είναι ο κόμβος που έχει τόσο μια σημείου-προς-σημείο συσκευή όσο και μια 
CSMA συσκευή. Θα δείτε ότι ο κόμβος δύο είναι ο πρώτος «επιπλέον» κόμβος στο CSMA
δίκτυο και ότι η συσκευή μηδέν του έχει επιλεγεί ως η συσκευή που θα καταγράψει τα 
ίχνη στην μεικτή κατάσταση.

..
	Now, let's follow the echo packet through the internetwork.  First, do a 
	tcpdump of the trace file for the leftmost point-to-point node --- node zero.
	
Τώρα, ας ακολουθήσουμε το echo πακέτο μέσα στο διαδίκτυο. Αρχικά, εκτελέστε το tcpdump 
του αρχείου ιχνών για τον αριστερότερο κόμβο σημείου-προς-σημείο --- τον κόμβο μηδέν.

.. sourcecode:: bash

  $ tcpdump -nn -tt -r second-0-0.pcap

.. You should see the contents of the pcap file displayed:\

Θα πρέπει να σας εμφανιστούν τα περιεχόμενα του αρχείου pcap:

.. sourcecode:: text

  reading from file second-0-0.pcap, link-type PPP (PPP)
  2.000000 IP 10.1.1.1.49153 > 10.1.2.4.9: UDP, length 1024
  2.017607 IP 10.1.2.4.9 > 10.1.1.1.49153: UDP, length 1024

..
	The first line of the dump indicates that the link type is PPP (point-to-point)
	which we expect.  You then see the echo packet leaving node zero via the 
	device associated with IP address 10.1.1.1 headed for IP address
	10.1.2.4 (the rightmost CSMA node).  This packet will move over the 
	point-to-point link and be received by the point-to-point net device on node 
	one.  Let's take a look:
	
Η πρώτη γραμμή του αποτελέσματος δείχνει ότι ο τύπος σύνδεσης είναι PPP (point-to-point ή
σημείο-προς-σημείο) κάτι το οποίο αναμέναμε. Έπειτα θα δείτε ότι το echo πακέτο φεύγει από
τον κόμβο μηδέν μέσω της συσκευής που αντιστοιχίζεται στην IP διεύθυνση 10.1.1.1 με κατεύθυνση
προς την IP διεύθυνση 10.1.2.4 (ο δεξιότερος κόμβος του CSMA). Αυτό το πακέτο θα κινηθεί πάνω από 
τη σύνδεση σημείου-προς-σημείο και θα παραληφθεί από τη δικτυακή συσκευή σημείου-προς-σημείο στον
κόμβο ένα. Ας ρίξουμε μια ματιά:

.. sourcecode:: bash

  $ tcpdump -nn -tt -r second-1-0.pcap

..
	You should now see the pcap trace output of the other side of the point-to-point
	link:
	
Θα πρέπει τώρα να βλέπετε το αποτέλεσμα των ιχνών pcap της άλλης πλευράς του συνδέσμου
σημείου-προς-σημείο:

.. sourcecode:: text

  reading from file second-1-0.pcap, link-type PPP (PPP)
  2.003686 IP 10.1.1.1.49153 > 10.1.2.4.9: UDP, length 1024
  2.013921 IP 10.1.2.4.9 > 10.1.1.1.49153: UDP, length 1024

..
	Here we see that the link type is also PPP as we would expect.  You see the
	packet from IP address 10.1.1.1 (that was sent at 2.000000 seconds) headed 
	toward IP address 10.1.2.4 appear on this interface.  Now, internally to this 
	node, the packet will be forwarded to the CSMA interface and we should see it 
	pop out on that device headed for its ultimate destination.
	
Εδώ βλέπουμε ότι ο τύπος σύνδεσης είναι επίσης PPP όπως θα περιμέναμε. Βλέπετε ότι το πακέτο
από την IP διεύθυνση 10.1.1.1 (το οποίο στάλθηκε τη χρονική στιγμή 2.000000 δευτερολέπτων) 
με κατεύθυνση προς την IP διεύθυνση 10.1.2.4 εμφανίστηκε στη διεπαφή αυτή. Τώρα, εσωτερικά σε 
αυτόν τον κόμβο, το πακέτο θα προωθηθεί στη διεπαφή CSMA και θα πρέπει να το δούμε να εξέρχεται
από τη συσκευή με κατεύθυνση προς τον τελικό του προορισμό.

..
	Remember that we selected node 2 as the promiscuous sniffer node for the CSMA
	network so let's then look at second-2-0.pcap and see if its there.
	
Θυμηθείτε ότι επιλέξαμε τον κόμβο δύο ως τον κόμβο που θα παρακολουθήσει (promiscuous sniffer) 
το CSMA δίκτυο, οπότε ας δούμε στο second-2-0.pcap για να διαπιστώσουμε αν είναι όντως εκεί.

.. sourcecode:: bash

  $ tcpdump -nn -tt -r second-2-0.pcap

.. You should now see the promiscuous dump of node two, device zero:

Θα πρέπει να βλέπετε τώρα τα αποτελέσματα για τον κόμβο δύο, και τη συσκευή μηδέν:

.. sourcecode:: text

  reading from file second-2-0.pcap, link-type EN10MB (Ethernet)
  2.007698 ARP, Request who-has 10.1.2.4 (ff:ff:ff:ff:ff:ff) tell 10.1.2.1, length 50
  2.007710 ARP, Reply 10.1.2.4 is-at 00:00:00:00:00:06, length 50
  2.007803 IP 10.1.1.1.49153 > 10.1.2.4.9: UDP, length 1024
  2.013815 ARP, Request who-has 10.1.2.1 (ff:ff:ff:ff:ff:ff) tell 10.1.2.4, length 50
  2.013828 ARP, Reply 10.1.2.1 is-at 00:00:00:00:00:03, length 50
  2.013921 IP 10.1.2.4.9 > 10.1.1.1.49153: UDP, length 1024

..
	As you can see, the link type is now "Ethernet".  Something new has appeared,
	though.  The bus network needs ``ARP``, the Address Resolution Protocol.
	Node one knows it needs to send the packet to IP address 10.1.2.4, but it
	doesn't know the MAC address of the corresponding node.  It broadcasts on the
	CSMA network (ff:ff:ff:ff:ff:ff) asking for the device that has IP address
	10.1.2.4.  In this case, the rightmost node replies saying it is at MAC address
	00:00:00:00:00:06.  Note that node two is not directly involved in this 
	exchange, but is sniffing the network and reporting all of the traffic it sees.
	
Όπως μπορείτε να δείτε, ο τύπος σύνδεσης είναι τώρα "Ethernet". Κάτι νέο έχει προκύψει,
ωστόσο. Το δίκτυο αρτηρίας χρειάζεται το ``ARP``, το Πρωτόκολλο Ανάλυσης Διευθύνσεων 
(Address Resolution Protocol). Ο κόμβος ένα γνωρίζει ότι χρειάζεται να στείλει το πακέτο
στην IP διεύθυνση 10.1.2.4, αλλά δεν ξέρει τη MAC διεύθυνση του αντίστοιχου κόμβου. Εκπέμπει
στο δίκτυο CSMA (ff:ff:ff:ff:ff:ff) αναζητώντας τη συσκευή με IP διεύθυνση 10.1.2.4. 
Σε αυτήν την περίπτωση, ο δεξιότερος κόμβος απαντάει λέγοντας ότι βρίσκεται στη MAC διεύθυνση
00:00:00:00:00:06. Σημειώστε ότι ο κόμβος δύο δεν εμπλέκεται άμεσα σε αυτήν την ανταλλαγή, αλλά
παρακολουθεί το δίκτυο και αναφέρει κάθε κίνηση που ανιχνεύει.

.. This exchange is seen in the following lines,

Η ανταλλαγή αυτή φαίνεται στις ακόλουθες γραμμές,

.. sourcecode:: text

  2.007698 ARP, Request who-has 10.1.2.4 (ff:ff:ff:ff:ff:ff) tell 10.1.2.1, length 50
  2.007710 ARP, Reply 10.1.2.4 is-at 00:00:00:00:00:06, length 50

..
	Then node one, device one goes ahead and sends the echo packet to the UDP echo
	server at IP address 10.1.2.4. 
	
Τότε η συσκευή ένα στον κόμβο ένα προχωρά και στέλνει το echo πακέτο στον UDP echo εξυπηρετητή
στην IP διεύθυνση 10.1.2.4.

.. sourcecode:: text

  2.007803 IP 10.1.1.1.49153 > 10.1.2.4.9: UDP, length 1024

..
	The server receives the echo request and turns the packet around trying to send
	it back to the source.  The server knows that this address is on another network
	that it reaches via IP address 10.1.2.1.  This is because we initialized global
	routing and it has figured all of this out for us.  But, the echo server node
	doesn't know the MAC address of the first CSMA node, so it has to ARP for it
	just like the first CSMA node had to do.
	
Ο εξυπηρετητής λαμβάνει το αίτημα echo και αντιστρέφει την κατεύθυνση του πακέτου προσπαθώντας
να το στείλει πίσω στην πηγή. Ο εξυπηρετητής γνωρίζει ότι αυτή η διεύθυνση είναι σε ένα άλλο
δίκτυο το οποίο μπορεί να προσπελάσει μέσω της IP διεύθυνσης 10.1.2.1. Αυτό συμβαίνει επειδή 
έχουμε ενεργοποιήσει την καθολική δρομολόγηση και τα έχει ξεκαθαρίσει όλα αυτά εκ μέρους μας. Αλλά,
ο κόμβος του echo εξυπηρετητή δεν γνωρίζει τη MAC διεύθυνση του πρώτου CSMA κόμβου, οπότε πρέπει να
εκτελέσει το πρωτόκολλο ARP, όπως χρειάστηκε να κάνει και ο πρώτος CSMA κόμβος.

.. sourcecode:: text

  2.013815 ARP, Request who-has 10.1.2.1 (ff:ff:ff:ff:ff:ff) tell 10.1.2.4, length 50
  2.013828 ARP, Reply 10.1.2.1 is-at 00:00:00:00:00:03, length 50

.. The server then sends the echo back to the forwarding node.

Ο εξυπηρετητής στέλνει τότε το echo πακέτο πίσω στον κόμβο προώθησης.

.. sourcecode:: text

  2.013921 IP 10.1.2.4.9 > 10.1.1.1.49153: UDP, length 1024

.. Looking back at the rightmost node of the point-to-point link,

Κοιτώντας πίσω στον δεξιότερο κόμβο του συνδέσμου σημείου-προς-σημείο,

.. sourcecode:: bash

  $ tcpdump -nn -tt -r second-1-0.pcap

..
	You can now see the echoed packet coming back onto the point-to-point link as
	the last line of the trace dump.
	
Μπορείτε να δείτε τώρα το πακέτο που έχει γίνει echo να επιστρέφει μέσω του συνδέσμου
σημείου-προς-σημείο στην τελευταία γραμμή των αποτελεσμάτων καταγραφής.

.. sourcecode:: text

  reading from file second-1-0.pcap, link-type PPP (PPP)
  2.003686 IP 10.1.1.1.49153 > 10.1.2.4.9: UDP, length 1024
  2.013921 IP 10.1.2.4.9 > 10.1.1.1.49153: UDP, length 1024

.. Lastly, you can look back at the node that originated the echo

Τέλος, μπορείτε να δείτε πίσω στον κόμβο που ξεκίνησε το echo

.. sourcecode:: bash

  $ tcpdump -nn -tt -r second-0-0.pcap

.. and see that the echoed packet arrives back at the source at 2.007602 seconds,

και να διαπιστώσετε ότι το πακέτο που έγινε echo φτάνει πίσω στην πηγή τη χρονική στιγμή
των 2.007602 δευτερολέπτων,

.. sourcecode:: text

  reading from file second-0-0.pcap, link-type PPP (PPP)
  2.000000 IP 10.1.1.1.49153 > 10.1.2.4.9: UDP, length 1024
  2.017607 IP 10.1.2.4.9 > 10.1.1.1.49153: UDP, length 1024

.. 
	Finally, recall that we added the ability to control the number of CSMA devices
	in the simulation by command line argument.  You can change this argument in
	the same way as when we looked at changing the number of packets echoed in the
	``first.cc`` example.  Try running the program with the number of "extra" 
	devices set to four:
	
Εν τέλει, θυμηθείτε ότι προσθέσαμε την δυνατότητα ελέγχου του αριθμού των CSMA συσκευών
στη προσομοίωση μέσω ορίσματος στη γραμμή εντολών. Μπορείτε να αλλάξετε αυτό το όρισμα με τον
ίδιο τρόπο όπως τότε που εξετάσαμε την αλλαγή του αριθμού των πακέτων που γίνονται echo στο 
παράδειγμα ``first.cc``. Δοκιμάστε να τρέξετε το πρόγραμμα με το αριθμό των «επιπλέον» συσκευών
να είναι ίσος με τέσσερεις:

.. sourcecode:: bash

  $ ./waf --run "scratch/mysecond --nCsma=4"

.. You should now see,

Θα πρέπει τώρα να βλέπετε τα εξής:

.. sourcecode:: text

  Waf: Entering directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  Waf: Leaving directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  'build' finished successfully (0.405s)
  At time 2s client sent 1024 bytes to 10.1.2.5 port 9
  At time 2.0118s server received 1024 bytes from 10.1.1.1 port 49153
  At time 2.0118s server sent 1024 bytes to 10.1.1.1 port 49153
  At time 2.02461s client received 1024 bytes from 10.1.2.5 port 9

..
	Notice that the echo server has now been relocated to the last of the CSMA
	nodes, which is 10.1.2.5 instead of the default case, 10.1.2.4.
	
Παρατηρήστε ότι ο εξυπηρετητής echo έχει επανατοποθετηθεί πλέον στον τελευταίο από τους CSMA κόμβους, ο οποίος είναι στη διεύθυνση 10.1.2.5 αντί της προεπιλεγμένης, 10.1.2.4.

..
	It is possible that you may not be satisfied with a trace file generated by
	a bystander in the CSMA network.  You may really want to get a trace from
	a single device and you may not be interested in any other traffic on the 
	network.  You can do this fairly easily.
	
Είναι πιθανό να μην είστε ικανοποιημένοι με ένα αρχείο καταγραφής που δημιουργείται
από έναν παρατηρητή στο CSMA δίκτυο. Μπορεί να θέλετε πραγματικά να λάβετε ίχνη από
μια μόνο συσκευή και να μην ενδιαφέρεστε για κάποια άλλη κίνηση στο δίκτυο. Αυτό
μπορείτε να το κάνετε αρκετά εύκολα.

..
	Let's take a look at ``scratch/mysecond.cc`` and add that code enabling us
	to be more specific.  ``ns-3`` helpers provide methods that take a node
	number and device number as parameters.  Go ahead and replace the 
	``EnablePcap`` calls with the calls below.
	
Ας δούμε το ``scratch/mysecond.cc`` και ας προσθέσουμε εκείνον τον κώδικα που θα μας 
επιτρέψει να είμαστε πιο συγκεκριμένοι. Οι βοηθοί του ``ns-3`` παρέχουν μεθόδους που δέχονται
έναν αριθμό κόμβου και έναν αριθμό συσκευής ως παραμέτρους. Προχωρήστε και αντικαταστήστε τις
κλήσεις ``EnablePcap`` με τις παρακάτω κλήσεις.

::

  pointToPoint.EnablePcap ("second", p2pNodes.Get (0)->GetId (), 0);
  csma.EnablePcap ("second", csmaNodes.Get (nCsma)->GetId (), 0, false);
  csma.EnablePcap ("second", csmaNodes.Get (nCsma-1)->GetId (), 0, false);

..
	We know that we want to create a pcap file with the base name "second" and
	we also know that the device of interest in both cases is going to be zero,
	so those parameters are not really interesting.
	
Γνωρίζουμε ότι θέλουμε να δημιουργήσουμε ένα αρχείο pcap με το βασικό όνομα "second", και
επίσης γνωρίζουμε ότι η συσκευή που μας ενδιαφέρει και στις δύο περιπτώσεις πρόκειται να είναι
η μηδέν, κατά συνέπεια οι παράμετροι δεν είναι και πολύ ενδιαφέρουσες.

..
	In order to get the node number, you have two choices:  first, nodes are 
	numbered in a monotonically increasing fashion starting from zero in the 
	order in which you created them.  One way to get a node number is to figure 
	this number out "manually" by contemplating the order of node creation.  
	If you take a look at the network topology illustration at the beginning of 
	the file, we did this for you and you can see that the last CSMA node is 
	going to be node number ``nCsma + 1``.  This approach can become 
	annoyingly difficult in larger simulations.  
	
Προκειμένου να βρείτε τον αριθμό του κόμβου, έχετε δύο επιλογές: πρώτον, οι κόμβοι είναι
αριθμημένοι με μονότονα αύξοντα τρόπο, ξεκινώντας από το μηδέν, με τη σειρά σύμφωνα με την
οποία τους δημιουργείτε. Ένας τρόπος για να πάρετε τον αριθμό ενός κόμβου είναι να βρείτε
τον αριθμό αυτό «μηχανικά», μελετώντας τη σειρά της δημιουργίας των κόμβων. Εάν ρίξετε μια ματιά
στην απεικόνιση της τοπολογίας του δικτύου στην αρχή του αρχείου, θα δείτε ότι το κάναμε ήδη αυτό
για εσάς, και θα διαπιστώσετε ότι ο τελευταίος CSMA κόμβος είναι ο κόμβος με τον αριθμό
``nCsma + 1``. Αυτή η προσέγγιση μπορεί να καταστεί ενοχλητικά δύσκολη σε μεγαλύτερες 
προσομοιώσεις.

..
	An alternate way, which we use here, is to realize that the
	``NodeContainers`` contain pointers to |ns3| ``Node`` Objects.
	The ``Node`` Object has a method called ``GetId`` which will return that
	node's ID, which is the node number we seek.  Let's go take a look at the 
	Doxygen for the ``Node`` and locate that method, which is further down in 
	the |ns3| core code than we've seen so far; but sometimes you have to
	search diligently for useful things.
	
Ένας εναλλακτικός τρόπος, τον οποίο χρησιμοποιούμε εδώ, είναι το να διαπιστώσετε ότι οι 
``NodeContainers`` περιέχουν δείκτες προς αντικείμενα ``Node`` του |ns3|. Το αντικείμενο
``Node`` έχει μια μέθοδο που ονομάζεται ``GetId``, η οποία επιστρέφει την ID του κόμβου,
η οποία είναι ο αριθμός του κόμβου που ψάχνουμε. Ας πάμε να δούμε στο Doxygen για το
``Node`` και ας εντοπίσουμε αυτή τη μέθοδο, η οποία βρίσκεται στο χαμηλότερο επίπεδο του
πυρήνα του |ns3| που έχουμε φτάσει μέχρι στιγμής. Κάποιες φορές θα χρειαστεί όντως να ψάξετε
με μεγάλη επιμέλεια για να βρείτε χρήσιμα πράγματα.

..
	Go to the Doxygen documentation for your release (recall that you can find it
	on the project web site).  You can get to the ``Node`` documentation by
	looking through at the "Classes" tab and scrolling down the "Class List" 
	until you find ``ns3::Node``.  Select ``ns3::Node`` and you will be taken
	to the documentation for the ``Node`` class.  If you now scroll down to the
	``GetId`` method and select it, you will be taken to the detailed 
	documentation for the method.  Using the ``GetId`` method can make 
	determining node numbers much easier in complex topologies.
	
Μεταβείτε στην τεκμηρίωση του Doxygen για την έκδοσή σας (θυμηθείτε ότι μπορείτε να τη βρείτε
στον ιστότοπο του project). Μπορείτε να βρείτε την τεκμηρίωση του ``Node`` ψάχνοντας στην
καρτέλα "Classes" και κατεβαίνοντας κάτω στην "Class List" μέχρι να βρείτε το ``ns3::Node``.
Επιλέξτε το ``ns3::Node`` και θα μεταβείτε στην τεκμηρίωση για την κλάση ``Node``. Εάν κατεβείτε
κάτω στη μέθοδο ``GetId`` και την επιλέξετε, θα μεταβείτε σε μια λεπτομερή τεκμηρίωση της μεθόδου
αυτής. Η χρήση της μεθόδου ``GetId`` μπορεί να κάνει τον προσδιορισμό του αριθμού ενός κόμβου
πολύ ευκολότερο σε πολύπλοκες τοπολογίες.

..
	Let's clear the old trace files out of the top-level directory to avoid confusion
	about what is going on,
	
Ας διαγράψουμε τα παλιά αρχεία καταγραφής από τον κατάλογο υψηλότερου επιπέδου, ώστε να αποφύγουμε
οποιαδήποτε σύγχυση σχετικά με το τι συμβαίνει.

.. sourcecode:: bash

  $ rm *.pcap
  $ rm *.tr

.. If you build the new script and run the simulation setting ``nCsma`` to 100,

Εάν κάνετε build το νέο σενάριο και εκτελέσετε την προσομοίωση θέτοντας τη μεταβλητή 
``nCsma`` στο 100,

.. sourcecode:: bash

  $ ./waf --run "scratch/mysecond --nCsma=100"

.. you will see the following output:

θα δείτε την ακόλουθη έξοδο:

.. sourcecode:: text

  Waf: Entering directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  Waf: Leaving directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  'build' finished successfully (0.407s)
  At time 2s client sent 1024 bytes to 10.1.2.101 port 9
  At time 2.0068s server received 1024 bytes from 10.1.1.1 port 49153
  At time 2.0068s server sent 1024 bytes to 10.1.1.1 port 49153
  At time 2.01761s client received 1024 bytes from 10.1.2.101 port 9

..
	Note that the echo server is now located at 10.1.2.101 which corresponds to
	having 100 "extra" CSMA nodes with the echo server on the last one.  If you
	list the pcap files in the top level directory you will see,
	
Σημειώστε ότι ο echo εξυπηρετητής βρίσκεται τώρα στη διεύθυνση 10.1.2.101, γεγονός που οφείλεται
στο ότι έχουμε 100 «επιπλέον» CSMA κόμβους, μαζί με τον echo εξυπηρετητή που βρίσκεται στον
τελευταίο από αυτούς. Εάν ζητήσετε τη λίστα με τα αρχεία pcap στον κατάλογο υψηλότερου επιπέδου
θα δείτε τα εξής:

.. sourcecode:: text

  second-0-0.pcap  second-100-0.pcap  second-101-0.pcap

..
	The trace file ``second-0-0.pcap`` is the "leftmost" point-to-point device
	which is the echo packet source.  The file ``second-101-0.pcap`` corresponds
	to the rightmost CSMA device which is where the echo server resides.  You may 
	have noticed that the final parameter on the call to enable pcap tracing on the 
	echo server node was false.  This means that the trace gathered on that node
	was in non-promiscuous mode.
	
Το αρχείο καταγραφής ``second-0-0.pcap`` είναι η «αριστερότερη» συσκευή σημείου-προς-σημείο,
η οποία είναι η πηγή του πακέτου echo. Το αρχείο ``second-101-0.pcap`` αντιστοιχεί στην
δεξιότερη CSMA συσκευή στην οποία βρίσκεται ο εξυπηρετητής echo. Μπορεί να παρατηρήσατε 
ότι η τελευταία παράμετρος κατά την κλήση για ενεργοποίηση της pcap καταγραφής στον κόμβο του
echo εξυπηρετητή είναι τεθειμένη ως ψευδής. Αυτό σημαίνει ότι τα ίχνη που συγκεντρώνονται σε
αυτόν τον κόμβο ήταν σε μη-μεικτή κατάσταση.

..
	To illustrate the difference between promiscuous and non-promiscuous traces, we
	also requested a non-promiscuous trace for the next-to-last node.  Go ahead and
	take a look at the ``tcpdump`` for ``second-100-0.pcap``.
	
Για να διευκρινίσουμε τη διαφορά μεταξύ μεικτών και μη-μεικτών ιχνών, ζητήσαμε επιπλέον 
ένα μη-μεικτό ίχνος για τον κόμβο δίπλα από τον τελευταίο. Ρίξτε μια ματιά στο ``tcpdump``
για το αρχείο ``second-100-0.pcap``.

.. sourcecode:: bash

  $ tcpdump -nn -tt -r second-100-0.pcap

..
	You can now see that node 100 is really a bystander in the echo exchange.  The
	only packets that it receives are the ARP requests which are broadcast to the
	entire CSMA network.
	
Μπορείτε τώρα να δείτε ότι ο κόμβος 100 είναι πράγματι ένας παρατηρητής κατά την ανταλλαγή echo. 
Τα μόνα πακέτα που λαμβάνει είναι οι αιτήσεις ARP οι οποίες εκπέμπονται σε όλο το CSMA δίκτυο.

.. sourcecode:: text

  reading from file second-100-0.pcap, link-type EN10MB (Ethernet)
  2.006698 ARP, Request who-has 10.1.2.101 (ff:ff:ff:ff:ff:ff) tell 10.1.2.1, length 50
  2.013815 ARP, Request who-has 10.1.2.1 (ff:ff:ff:ff:ff:ff) tell 10.1.2.101, length 50

.. Now take a look at the ``tcpdump`` for ``second-101-0.pcap``.

Δείτε τώρα το ``tcpdump`` για το αρχείο ``second-101-0.pcap``.

.. sourcecode:: bash

  $ tcpdump -nn -tt -r second-101-0.pcap

.. You can now see that node 101 is really the participant in the echo exchange.

Μπορείτε τώρα να δείτε ότι ο κόμβος 101 είναι πραγματικά ο παραλήπτης της ανταλλαγής echo.

.. sourcecode:: text

  reading from file second-101-0.pcap, link-type EN10MB (Ethernet)
  2.006698 ARP, Request who-has 10.1.2.101 (ff:ff:ff:ff:ff:ff) tell 10.1.2.1, length 50
  2.006698 ARP, Reply 10.1.2.101 is-at 00:00:00:00:00:67, length 50
  2.006803 IP 10.1.1.1.49153 > 10.1.2.101.9: UDP, length 1024
  2.013803 ARP, Request who-has 10.1.2.1 (ff:ff:ff:ff:ff:ff) tell 10.1.2.101, length 50
  2.013828 ARP, Reply 10.1.2.1 is-at 00:00:00:00:00:03, length 50
  2.013828 IP 10.1.2.101.9 > 10.1.1.1.49153: UDP, length 1024

.. Models, Attributes and Reality

Μοντέλα, Χαρακτηριστικά και Πραγματικότητα
******************************************

..
	This is a convenient place to make a small excursion and make an important
	point.  It may or may not be obvious to you, but whenever one is using a 
	simulation, it is important to understand exactly what is being modeled and
	what is not.  It is tempting, for example, to think of the CSMA devices 
	and channels used in the previous section as if they were real Ethernet 
	devices; and to expect a simulation result to directly reflect what will 
	happen in a real Ethernet.  This is not the case.  
	
Αυτό το σημείο ενδείκνυται ώστε να κάνουμε μια μικρή παρένθεση και να σημειώσουμε κάτι
σημαντικό. Μπορεί να είναι φανερό σε εσάς ή μπορεί και όχι, μα όποτε κάποιος πραγματοποιεί
μια προσομοίωση, είναι σημαντικό να καταλαβαίνει ακριβώς τι μοντελοποιείται και τι όχι. Είναι
δελεαστικό, για παράδειγμα, να σκεφτεί κανείς ότι οι CSMA συσκευές και τα κανάλια που 
χρησιμοποιήθηκαν στην προηγούμενη ενότητα είναι σαν αληθινές συσκευές Ethernet. Και να περιμένει
ότι το αποτέλεσμα μιας προσομοίωσης θα αντανακλά άμεσα το τι θα συμβεί σε ένα πραγματικό Ethernet.
Δεν είναι, όμως, έτσι τα πράγματα.

..
	A model is, by definition, an abstraction of reality.  It is ultimately the 
	responsibility of the simulation script author to determine the so-called
	"range of accuracy" and "domain of applicability" of the simulation as
	a whole, and therefore its constituent parts.
	
Ένα μοντέλο είναι, εξ ορισμού, μια αφαίρεση της πραγματικότητας. Είναι τελικά στην
κρίση του συγγραφέα του σεναρίου προσομοίωσης το να καθορίσει το λεγόμενο «εύρος της ακρίβειας»
και «την περιοχή της εφαρμοσιμότητας» της προσομοίωσης συνολικά, και κατά συνέπεια των 
συστατικών μερών της.

..
	In some cases, like ``Csma``, it can be fairly easy to determine what is 
	*not* modeled.  By reading the model description (``csma.h``) you 
	can find that there is no collision detection in the CSMA model and decide
	on how applicable its use will be in your simulation or what caveats you 
	may want to include with your results.  In other cases, it can be quite easy
	to configure behaviors that might not agree with any reality you can go out
	and buy.  It will prove worthwhile to spend some time investigating a few
	such instances, and how easily you can swerve outside the bounds of reality
	in your simulations.
	
Σε μερικές περιπτώσεις, όπως στο ``Csma``, μπορεί να είναι σχετικά εύκολο το να καθοριστεί
τι *δεν* μπορεί να μοντελοποιηθεί. Διαβάζοντας την περιγραφή του μοντέλου (``csma.h``) μπορείτε
να διαπιστώσετε ότι δεν υπάρχει ανίχνευση συγκρούσεων στο CSMA μοντέλο, και να αποφασίσετε το
κατά πόσο εφαρμόσιμη θα είναι η χρήση του στην προσομοίωσή σας ή τι προειδοποιήσεις μπορεί 
να θέλετε να συμπεριλάβετε στα αποτελέσματά σας. Σε άλλες περιπτώσεις, μπορεί να είναι
αρκετά εύκολη η ρύθμιση συμπεριφορών που ενδέχεται να μη συμβαδίζουν με την πραγματικότητα. Το να αφιερώσετε χρόνο εξετάζοντας μερικές τέτοιες περιπτώσεις θα
αποδειχτεί ότι αξίζει τον κόπο, καθώς και το να εξετάσετε πόσο εύκολα μπορείτε να παρεκκλίνετε 
εκτός των ορίων της πραγματικότητας στις προσομοιώσεις σας.

..
	As you have seen, |ns3| provides ``Attributes`` which a user
	can easily set to change model behavior.  Consider two of the ``Attributes``
	of the ``CsmaNetDevice``:  ``Mtu`` and ``EncapsulationMode``.  
	The ``Mtu`` attribute indicates the Maximum Transmission Unit to the 
	device.  This is the size of the largest Protocol Data Unit (PDU) that the
	device can send. 
	
Όπως θα έχετε δει, ο |ns3| παρέχει ``Attributes`` τα οποία ένας χρήστης μπορεί εύκολα 
να θέσει ώστε να αλλάξει τη συμπεριφορά του μοντέλου. Θεωρήστε δύο από τα ``Attributes`` της 
``CsmaNetDevice``: το ``Mtu`` και το ``EncapsulationMode``. Το ``Mtu`` χαρακτηριστικό υποδηλώνει
τη Μέγιστη Μονάδα Μετάδοσης (Maximum Transmission Unit) της συσκευής. Είναι το μέγεθος της
μεγαλύτερης Μονάδας Δεδομένων του Πρωτοκόλλου (Protocol Data Unit ή PDU) που μπορεί να στείλει
η συσκευή.

..
	The MTU defaults to 1500 bytes in the ``CsmaNetDevice``.  This default
	corresponds to a number found in RFC 894, "A Standard for the Transmission
	of IP Datagrams over Ethernet Networks."  The number is actually derived 
	from the maximum packet size for 10Base5 (full-spec Ethernet) networks -- 
	1518 bytes.  If you subtract the DIX encapsulation overhead for Ethernet 
	packets (18 bytes) you will end up with a maximum possible data size (MTU) 
	of 1500 bytes.  One can also find that the ``MTU`` for IEEE 802.3 networks
	is 1492 bytes.  This is because LLC/SNAP encapsulation adds an extra eight 
	bytes of overhead to the packet.  In both cases, the underlying hardware can
	only send 1518 bytes, but the data size is different.
	
Το MTU είναι εξ ορισμού στα 1500 byte στην ``CsmaNetDevice``. Αυτή η προεπιλογή
αντιστοιχεί σε έναν αριθμό που υπάρχει στο πρότυπο RFC 894, "A Standard for the Transmission
of IP Datagrams over Ethernet Networks". Ο αριθμός προέρχεται όντως από το μέγιστο
μέγεθος πακέτου για δίκτυα τύπου 10Base5 (full-spec Ethernet) -- 1518 byte. Εάν αφαιρέσετε 
το πλεόνασμα της DIX ενθυλάκωσης (DIX encapsulation overhead) για τα Ethernet πακέτα 
(18 byte) θα καταλήξετε να έχετε μέγιστο δυνατό μέγεθος δεδομένων (MTU) ίσο με 1500 byte. 
Κάποιοι μπορεί να παρατηρήσουν ότι το ``MTU`` για δίκτυα IEEE 802.3 είναι 1492 byte. Αυτό
συμβαίνει επειδή η ενθυλάκωση LLC/SNAP προσθέτει ένα επιπρόσθετο βάρος από byte στο πλεόνασμα του
πακέτου. Και στις δύο περιπτώσεις, το υποκείμενο υλικό μπορεί να στείλει μόνο 1518 byte, αλλά το
μέγεθος των δεδομένων είναι διαφορετικό.

..
	In order to set the encapsulation mode, the ``CsmaNetDevice`` provides
	an ``Attribute`` called ``EncapsulationMode`` which can take on the 
	values ``Dix`` or ``Llc``.  These correspond to Ethernet and LLC/SNAP
	framing respectively.
	
Προκειμένου να καθορίσουμε την κατάσταση ενθυλάκωσης, η ``CsmaNetDevice`` παρέχει ένα
``Attribute`` που καλείται ``EncapsulationMode``, το οποίο μπορεί να πάρει τις 
τιμές ``Dix`` ή ``Llc``. Αυτές αντιστοιχούν στην πλαισίωση Ethernet και LLC/SNAP
κατ' αναλογία.

..
	If one leaves the ``Mtu`` at 1500 bytes and changes the encapsulation mode
	to ``Llc``, the result will be a network that encapsulates 1500 byte PDUs
	with LLC/SNAP framing resulting in packets of 1526 bytes, which would be 
	illegal in many networks, since they can transmit a maximum of 1518 bytes per
	packet.  This would most likely result in a simulation that quite subtly does
	not reflect the reality you might be expecting.
	
Αν αφήσει κάποιος το ``Mtu`` στα 1500 byte και αλλάξει την κατάσταση ενθυλάκωσης σε ``Llc``,
το αποτέλεσμα θα είναι ένα δίκτυο το οποίο ενθυλακώνει PDU των 1500 byte σε LLC/SNAP πλαίσια, 
που έχει σα συνέπεια την ύπαρξη πακέτων των 1526 byte, κάτι που θα ήταν ανεπιτρεπτό σε πολλά δίκτυα,
καθώς αυτά μπορούν να μεταδώσουν το πολύ 1518 byte ανά πακέτο. Πιθανότατα αυτό να είχε ως
αποτέλεσμα μια προσομοίωση που κατά περίεργο τρόπο δεν θα αντανακλά την πραγματικότητα που 
μπορεί εσείς να περιμένατε. 

..
	Just to complicate the picture, there exist jumbo frames (1500 < MTU <= 9000 bytes)
	and super-jumbo (MTU > 9000 bytes) frames that are not officially sanctioned
	by IEEE but are available in some high-speed (Gigabit) networks and NICs.  One
	could leave the encapsulation mode set to ``Dix``, and set the ``Mtu``
	``Attribute`` on a ``CsmaNetDevice`` to 64000 bytes -- even though an 
	associated ``CsmaChannel DataRate`` was set at 10 megabits per second.  
	This would essentially model an Ethernet switch made out of vampire-tapped
	1980s-style 10Base5 networks that support super-jumbo datagrams.  This is
	certainly not something that was ever made, nor is likely to ever be made,
	but it is quite easy for you to configure.
	
Για να κάνουμε πιο περίπλοκη την όλη υπόθεση, υπάρχουν τεράστια πλαίσια (1500 < MTU <= 9000 byte)
και υπερ-τεράστια (MTU > 9000 byte) πλαίσια που δεν είναι επίσημα επικυρωμένα από την IEEE, αλλά
είναι διαθέσιμα για κάποια δίκτυα υψηλών ταχυτήτων (Gigabit) και NIC. Κάποιος θα μπορούσε να 
αφήσει την κατάσταση ενθυλάκωσης στην επιλογή ``Dix``, και να θέσει το ``Attribute`` ``Mtu`` 
σε μια ``CsmaNetDevice`` στα 64000 byte -- ακόμα και αν ένα σχετικό ``CsmaChannel DataRate`` 
ήταν καθορισμένο στα 10 megabit ανά δευτερόλεπτο. Αυτό θα μοντελοποιούσε στην ουσία έναν διακόπτη
Ethernet, φτιαγμένο από δίκτυα 10Base5 της δεκαετίας του 1980 συνδεδεμένα με συσκευές vampire tap
που υποστηρίζουν υπερ-τεράστια πακέτα. Αυτό σίγουρα δεν είναι κάτι που έχει φτιαχτεί στο παρελθόν,
ούτε και πρόκειται να φτιαχτεί, αλλά είναι κάτι που μπορείτε πολύ εύκολα να ρυθμίσετε εσείς.

..
	In the previous example, you used the command line to create a simulation that
	had 100 ``Csma`` nodes.  You could have just as easily created a simulation
	with 500 nodes.  If you were actually modeling that 10Base5 vampire-tap network,
	the maximum length of a full-spec Ethernet cable is 500 meters, with a minimum 
	tap spacing of 2.5 meters.  That means there could only be 200 taps on a 
	real network.  You could have quite easily built an illegal network in that
	way as well.  This may or may not result in a meaningful simulation depending
	on what you are trying to model.
	
Στο προηγούμενο παράδειγμα, χρησιμοποιήσατε τη γραμμή εντολών για να δημιουργήσετε μια
προσομοίωση η οποία είχε 100 ``Csma`` κόμβους. Θα μπορούσατε το ίδιο απλά να έχετε 
δημιουργήσει μια προσομοίωση με 500 κόμβους. Εάν μοντελοποιούσατε πραγματικά το παραπάνω δίκτυο
10Base5 με τις vampire tap συσκευές, το μέγιστο μήκος ενός full-spec Ethernet καλωδίου είναι 
500 μέτρα, με ελάχιστη απόσταση μεταξύ συσκευών τα 2.5 μέτρα. Κάτι που σημαίνει ότι θα μπορούσαν
να υπάρχουν μόνο 200 τέτοιες συσκευές σε ένα πραγματικό δίκτυο. Επίσης, θα μπορούσατε αρκετά εύκολα
να φτιάξετε και ένα δίκτυο εκτός των προτύπων με τον ίδιο τρόπο. Κάτι τέτοιο μπορεί να έχει ή να 
μην έχει έχει ως αποτέλεσμα μια ουσιαστική προσομοίωση, δεδομένου και του τι προσπαθείτε
να μοντελοποιήσετε.

..
	Similar situations can occur in many places in |ns3| and in any
	simulator.  For example, you may be able to position nodes in such a way that
	they occupy the same space at the same time, or you may be able to configure
	amplifiers or noise levels that violate the basic laws of physics.
	
Παρόμοιες καταστάσεις μπορούν να προκύψουν σε πολλά σημεία στον |ns3| και σε οποιαδήποτε
προσομοίωση. Για παράδειγμα, μπορεί να είστε σε θέση να τοποθετήσετε κόμβους με τέτοιο τρόπο
ώστε να καταλαμβάνουν τον ίδιο χώρο την ίδια στιγμή, ή μπορεί να είστε σε θέση να ρυθμίσετε
του ενισχυτές ή τα επίπεδα θορύβου ώστε να παραβιάζουν τους βασικούς νόμους της Φυσικής.

..
	|ns3| generally favors flexibility, and many models will allow freely
	setting ``Attributes`` without trying to enforce any arbitrary consistency
	or particular underlying spec.
	
Ο |ns3| ευνοεί γενικά την ευελιξία, και πολλά μοντέλα θα σας επιτρέψουν να θέσετε
ελεύθερα ``Attributes`` χωρίς να προσπαθήσουν να σας επιβάλλουν οποιαδήποτε αυθαίρετη
συνοχή ή συγκεκριμένη υποκείμενη προδιαγραφή.

..
	The thing to take home from this is that |ns3| is going to provide a
	super-flexible base for you to experiment with.  It is up to you to understand
	what you are asking the system to do and to  make sure that the simulations you
	create have some meaning and some connection with a reality defined by you.
	
Αυτό που θα πρέπει να κρατήσετε εσείς είναι ότι ο |ns3| θα σας παρέχει μια υπερ-ευέλικτη βάση 
πάνω στην οποία μπορείτε να πειραματιστείτε. Εξαρτάται από εσάς το αν θα κατανοήσετε τι 
ζητάτε από το σύστημα να κάνει και αν θα διασφαλίσετε ότι οι προσομοιώσεις που δημιουργείτε
έχουν κάποιο νόημα και κάποια σύνδεση με κάποια πραγματικότητα που καθορίζεται από εσάς.

.. Building a Wireless Network Topology

Δημιουργώντας μια Τοπολογία Ασύρματου Δικτύου
*********************************************

..
	In this section we are going to further expand our knowledge of |ns3|
	network devices and channels to cover an example of a wireless network.  
	|ns3| provides a set of 802.11 models that attempt to provide an 
	accurate MAC-level implementation of the 802.11 specification and a 
	"not-so-slow" PHY-level model of the 802.11a specification.
	
Σε αυτήν την ενότητα θα επεκτείνουμε τις γνώσεις μας για τις δικτυακές συσκευές και τα 
κανάλια του |ns3| ώστε να καλύψουμε ένα παράδειγμα ενός ασύρματου δικτύου. Ο |ns3| παρέχει
ένα σύνολο από μοντέλα τύπου 802.11 που επιχειρούν να παρέχουν μια ακριβή υλοποίηση MAC-επιπέδου
των προδιαγραφών του 802.11, και ένα «όχι-και-τόσο-αργό» μοντέλο φυσικού επιπέδου σύμφωνα με 
τις προδιαγραφές του 802.11a.

..
	Just as we have seen both point-to-point and CSMA topology helper objects when
	constructing point-to-point topologies, we will see equivalent ``Wifi``
	topology helpers in this section.  The appearance and operation of these 
	helpers should look quite familiar to you.
	
Ακριβώς όπως έχουμε δει στα αντικείμενα βοηθών τοπολογίας και σημείου-προς-σημείο και CSMA όταν 
κατασκευάζαμε τοπολογίες σημείου-προς σημείο, θα δούμε ανάλογους βοηθούς τοπολογίας ``Wifi`` σε
αυτήν την ενότητα. Η εμφάνιση και η λειτουργία αυτών των βοηθών θα πρέπει να σας είναι αρκετά
οικεία.

..
	We provide an example script in our ``examples/tutorial`` directory.  This script
	builds on the ``second.cc`` script and adds a Wifi network.  Go ahead and
	open ``examples/tutorial/third.cc`` in your favorite editor.  You will have already
	seen enough |ns3| code to understand most of what is going on in 
	this example, but there are a few new things, so we will go over the entire 
	script and examine some of the output.
	
Σας παρέχουμε ένα παραδειγματικό σενάριο στον κατάλογο ``examples/tutorial``. Το σενάριο αυτό
βασίζεται πάνω στο σενάριο ``second.cc`` και προσθέτει ένα δίκτυο Wifi. Προχωρήστε και ανοίξτε
το αρχείο ``examples/tutorial/third.cc`` στον επεξεργαστή κειμένου της προτίμησής σας. Θα έχετε
ήδη δει αρκετό κώδικα του |ns3| ώστε πλέον να μπορείτε να καταλάβετε τα περισσότερα από αυτά που 
συμβαίνουν σε αυτό το παράδειγμα, αλλά υπάρχουν και μερικά νέα πράγματα, οπότε θα περιηγηθούμε κατά
μήκος ολόκληρου του σενάριου και θα εξετάσουμε κάποια από τα αποτελέσματά του. 

..
	Just as in the ``second.cc`` example (and in all |ns3| examples)
	the file begins with an emacs mode line and some GPL boilerplate.
	
Όπως και στο παράδειγμα ``second.cc`` (και σε όλα τα παραδείγματα του |ns3|) το αρχείο 
ξεκινά με μια γραμμή κατάστασης για τον emacs και κάποιες κοινές δηλώσεις GPL.

..
	Take a look at the ASCII art (reproduced below) that shows the default network
	topology constructed in the example.  You can see that we are going to 
	further extend our example by hanging a wireless network off of the left side.
	Notice that this is a default network topology since you can actually vary the
	number of nodes created on the wired and wireless networks.  Just as in the 
	``second.cc`` script case, if you change ``nCsma``, it will give you a 
	number of "extra" CSMA nodes.  Similarly, you can set ``nWifi`` to 
	control how many ``STA`` (station) nodes are created in the simulation.
	There will always be one ``AP`` (access point) node on the wireless 
	network.  By default there are three "extra" CSMA nodes and three wireless 
	``STA`` nodes.
	
Ρίξτε μια ματιά στην ASCII τέχνη (που παρατίθεται παρακάτω), η οποία δείχνει την
προεπιλεγμένη τοπολογία δικτύου που κατασκευάζεται στο παράδειγμα. Μπορείτε να δείτε ότι 
πρόκειται να επεκτείνουμε το παράδειγμά μας συνδέοντας ένα ασύρματο δίκτυο στην αριστερή
πλευρά. Παρατηρήστε ότι αυτή είναι μια προεπιλεγμένη τοπολογία δικτύου, καθώς μπορείτε
στην ουσία να αλλάξετε τον αριθμό των κόμβων που δημιουργούνται στα ενσύρματα και ασύρματα
δίκτυα. Όπως και στην περίπτωση του σεναρίου ``second.cc``, εάν αλλάξετε τη ``nCsma``, θα σας
δώσει έναν αριθμό από «επιπλέον« CSMA κόμβους. Με παρόμοιο τρόπο, μπορείτε να θέσετε
τη μεταβλητή ``nWifi`` ώστε να ελέγχετε πόσοι ``STA`` κόμβοι (station ή σταθμοί) θα δημιουργηθούν
στην προσομοίωση. Πάντα θα υπάρχει ένας κόμβος ``AP`` (access point ή σημείο πρόσβασης) στο
ασύρματο δίκτυο. Εξ ορισμού, υπάρχουν τρεις «επιπλέον» CSMA κόμβου και τρεις ασύρματοι ``STA``
κόμβοι. 

..
	The code begins by loading module include files just as was done in the
	``second.cc`` example.  There are a couple of new includes corresponding
	to the Wifi module and the mobility module which we will discuss below.
	
Ο κώδικας ξεκινά με τη φόρτωση αρχείων συμπερίληψης ενοτήτων, όπως έγινε και στο παράδειγμα
``second.cc``. Υπάρχουν μερικές νέες συμπεριλήψεις κώδικα που αντιστοιχούν στην ενότητα του
Wifi και στην ενότητα της κινητικότητας (mobility), για τις οποίες θα πούμε παρακάτω.

::

#include "ns3/core-module.h"
#include "ns3/point-to-point-module.h"
#include "ns3/network-module.h"
#include "ns3/applications-module.h"
#include "ns3/wifi-module.h"
#include "ns3/mobility-module.h"
#include "ns3/csma-module.h"
#include "ns3/internet-module.h"

.. The network topology illustration follows:

Ακολουθεί η απεικόνιση της τοπολογίας δικτύου: 

::

  // Default Network Topology
  //
  //   Wifi 10.1.3.0
  //                 AP
  //  *    *    *    *
  //  |    |    |    |    10.1.1.0
  // n5   n6   n7   n0 -------------- n1   n2   n3   n4
  //                   point-to-point  |    |    |    |
  //                                   ================
  //                                     LAN 10.1.2.0

..
	You can see that we are adding a new network device to the node on the left 
	side of the point-to-point link that becomes the access point for the wireless
	network.  A number of wireless STA nodes are created to fill out the new 
	10.1.3.0 network as shown on the left side of the illustration.
	
Μπορείτε να δείτε ότι προσθέτουμε μια καινούργια δικτυακή συσκευή στον κόμβο στην αριστερή
πλευρά της σύνδεσης σημείου-προς-σημείο, η οποία γίνεται το σημείο πρόσβασης για το ασύρματο
δίκτυο. Ένας αριθμός από ασύρματους STA κόμβους δημιουργείται ώστε να γεμίσει το νέο
δίκτυο με διεύθυνση 10.1.3.0, όπως φαίνεται στην αριστερή πλευρά της απεικόνισης.

..
	After the illustration, the ``ns-3`` namespace is ``used`` and a logging
	component is defined.  This should all be quite familiar by now.
	
Μετά την απεικόνιση, ``χρησιμοποιείται`` ο χώρος ονομάτων του ``ns-3`` και ορίζεται
ένα στοιχείο καταγραφής. Όλα αυτά θα πρέπει να σας είναι αρκετά οικεία πλέον.

::

  using namespace ns3;
  
  NS_LOG_COMPONENT_DEFINE ("ThirdScriptExample");

..
	The main program begins just like ``second.cc`` by adding some command line
	parameters for enabling or disabling logging components and for changing the 
	number of devices created.
	
Το κυρίως πρόγραμμα ξεκινά όπως και στο ``second.cc`` με την προσθήκη μερικών 
παραμέτρων της γραμμής εντολών για την ενεργοποίηση ή απενεργοποίηση των στοιχείων καταγραφής
και για την αλλαγή του αριθμού των συσκευών που δημιουργούνται.

::

  bool verbose = true;
  uint32_t nCsma = 3;
  uint32_t nWifi = 3;

  CommandLine cmd;
  cmd.AddValue ("nCsma", "Number of \"extra\" CSMA nodes/devices", nCsma);
  cmd.AddValue ("nWifi", "Number of wifi STA devices", nWifi);
  cmd.AddValue ("verbose", "Tell echo applications to log if true", verbose);

  cmd.Parse (argc,argv);

  if (verbose)
    {
      LogComponentEnable("UdpEchoClientApplication", LOG_LEVEL_INFO);
      LogComponentEnable("UdpEchoServerApplication", LOG_LEVEL_INFO);
    }

..
	Just as in all of the previous examples, the next step is to create two nodes
	that we will connect via the point-to-point link.  
	
Όπως και σε όλα τα προηγούμενα παραδείγματα, το επόμενο βήμα είναι η δημιουργία δύο κόμβων
που θα ενώνονται μέσω ενός συνδέσμου σημείου-προς-σημείο.

::

  NodeContainer p2pNodes;
  p2pNodes.Create (2);

..
	Next, we see an old friend.  We instantiate a ``PointToPointHelper`` and 
	set the associated default ``Attributes`` so that we create a five megabit 
	per second transmitter on devices created using the helper and a two millisecond 
	delay on channels created by the helper.  We then ``Intall`` the devices
	on the nodes and the channel between them.
	
Έπειτα, συναντάμε έναν παλιό μας γνώριμο. Δημιουργούμε έναν ``PointToPointHelper`` και
θέτουμε τα σχετικά προεπιλεγμένα ``Attributes``, έτσι ώστε να δημιουργήσουμε έναν πομπό
με ταχύτητα μετάδοσης πέντε megabit ανά δευτερόλεπτο πάνω στις συσκευές που δημιουργήσαμε
με τη βοήθεια του βοηθού, και για να ορίσουμε καθυστέρηση δύο μιλιδευτερολέπτων στα κανάλια
που δημιουργήθηκαν από τον βοηθό. Έπειτα εγκαθιστούμε τις συσκευές στους κόμβους
και τα κανάλια ανάμεσά τους.

::

  PointToPointHelper pointToPoint;
  pointToPoint.SetDeviceAttribute ("DataRate", StringValue ("5Mbps"));
  pointToPoint.SetChannelAttribute ("Delay", StringValue ("2ms"));

  NetDeviceContainer p2pDevices;
  p2pDevices = pointToPoint.Install (p2pNodes);

..
	Next, we declare another ``NodeContainer`` to hold the nodes that will be
	part of the bus (CSMA) network.
	
Στη συνέχεια, δηλώνουμε άλλον ένα ``NodeContainer``, προκειμένου να περιέχει τους κόμβους που θα 
είναι μέρος του δικτύου αρτηρίας (CSMA).

::

  NodeContainer csmaNodes;
  csmaNodes.Add (p2pNodes.Get (1));
  csmaNodes.Create (nCsma);

..
	The next line of code ``Gets`` the first node (as in having an index of one)
	from the point-to-point node container and adds it to the container of nodes
	that will get CSMA devices.  The node in question is going to end up with a 
	point-to-point device and a CSMA device.  We then create a number of "extra"
	nodes that compose the remainder of the CSMA network.
	
Η επόμενη γραμμή κώδικα παίρνει (``Gets``) τον πρώτο κόμβο (δηλαδή σα να έχει ένα ευρετήριο
που να περιέχει έναν) από τον container των κόμβων σημείου-προς-σημείο και τον προσθέτει στον
container των κόμβων οι οποίοι θα δεχτούν μετέπειτα τις CSMA συσκευές. Ο εν λόγω κόμβος πρόκειται
να καταλήξει να έχει μια συσκευή σημείου-προς-σημείο και μια CSMA συσκευή. Έπειτα δημιουργούμε
έναν αριθμός από «επιπλέον» κόμβους, οι οποίοι συνθέτουν το υπόλοιπο του CSMA δικτύου.

..
	We then instantiate a ``CsmaHelper`` and set its ``Attributes`` as we did
	in the previous example.  We create a ``NetDeviceContainer`` to keep track of
	the created CSMA net devices and then we ``Install`` CSMA devices on the 
	selected nodes.
	
Έπειτα δημιουργούμε έναν ``CsmaHelper`` και θέτουμε τα ``Attributes`` του όπως κάναμε 
και στο προηγούμενο παράδειγμα. Δημιουργούμε ένα ``NetDeviceContainer`` για να καταγράψουμε
τις δικτυακές συσκευές CSMA που δημιουργήθηκαν και στη συνέχεια εγκαθιστούμε τις CSMA 
συσκευές στους επιλεγμένους κόμβους.

::

  CsmaHelper csma;
  csma.SetChannelAttribute ("DataRate", StringValue ("100Mbps"));
  csma.SetChannelAttribute ("Delay", TimeValue (NanoSeconds (6560)));

  NetDeviceContainer csmaDevices;
  csmaDevices = csma.Install (csmaNodes);

..
	Next, we are going to create the nodes that will be part of the Wifi network.
	We are going to create a number of "station" nodes as specified by the 
	command line argument, and we are going to use the "leftmost" node of the 
	point-to-point link as the node for the access point.
	
Μετά πρόκειται να δημιουργήσουμε τους κόμβους που θα είναι μέρος του Wifi δικτύου. Θα
δημιουργησουμε έναν αριθμό από κόμβους-«σταθμούς», όπως προσδιορίζεται από το όρισμα στη
γραμμή εντολών, και θα χρησιμοποιήσουμε τον «αριστερότερο» κόμβο του συνδέσμου
σημείου-προς-σημείο ως τον κόμβο για το σημείο πρόσβασης.

::

  NodeContainer wifiStaNodes;
  wifiStaNodes.Create (nWifi);
  NodeContainer wifiApNode = p2pNodes.Get (0);

..
	The next bit of code constructs the wifi devices and the interconnection
	channel between these wifi nodes. First, we configure the PHY and channel
	helpers:
	
Το επόμενο κομμάτι κώδικα κατασκευάζει τις συσκευές wifi και το διασυνδετικό κανάλι
ανάμεσα σε αυτούς τους wifi κόμβους. Αρχικά, ρυθμίζει τους βοηθούς φυσικού επιπέδου (PHY)
και καναλιού: 

::

  YansWifiChannelHelper channel = YansWifiChannelHelper::Default ();
  YansWifiPhyHelper phy = YansWifiPhyHelper::Default ();

..
	For simplicity, this code uses the default PHY layer configuration and
	channel models which are documented in the API doxygen documentation for
	the ``YansWifiChannelHelper::Default`` and ``YansWifiPhyHelper::Default``
	methods. Once these objects are created, we create a channel object
	and associate it to our PHY layer object manager to make sure
	that all the PHY layer objects created by the ``YansWifiPhyHelper``
	share the same underlying channel, that is, they share the same
	wireless medium and can communication and interfere:
	
Για λόγους απλότητας, αυτός ο κώδικας χρησιμοποιεί την προεπιλεγμένη ρύθμιση του φυσικού
επιπέδου και μοντέλα καναλιού που έχουν τεκμηριωθεί στην τεκμηρίωση API στο Doxygen για τις
μεθόδους ``YansWifiChannelHelper::Default`` και ``YansWifiPhyHelper::Default``. Μόλις
δημιουργηθούν αυτά τα αντικείμενα, δημιουργούμε ένα αντικείμενο καναλιού και το
συσχετίζουμε με τον διαχειριστή αντικειμένων μας του φυσικού επιπέδου, ώστε να σιγουρευτούμε
ότι όλα τα αντικείμενα που δημιουργούνται στο φυσικό επίπεδο από τον ``YansWifiPhyHelper``
μοιράζονται το ίδιο βασικό κανάλι, που σημαίνει ότι μοιράζονται το ίδιο ασύρματο μέσο
και μπορούν να επικοινωνήσουν και να παρέμβουν:

::

  phy.SetChannel (channel.Create ());

..
	Once the PHY helper is configured, we can focus on the MAC layer. Here we choose to
	work with non-Qos MACs so we use a NqosWifiMacHelper object to set MAC parameters. 
	
Μόλις ρυθμιστεί και ο βοηθός φυσικού επιπέδου (PHY), μπορούμε να επικεντρωθούμε στο MAC επίπεδο. 
Εδώ επιλέγουμε να δουλέψουμε με non-Qos MAC, οπότε χρησιμοποιούμε ένα αντικείμενο NqosWifiMacHelper
για να θέσουμε τις παραμέτρους που αφορούν το MAC.

::

  WifiHelper wifi = WifiHelper::Default ();
  wifi.SetRemoteStationManager ("ns3::AarfWifiManager");

  NqosWifiMacHelper mac = NqosWifiMacHelper::Default ();

..
	The ``SetRemoteStationManager`` method tells the helper the type of 
	rate control algorithm to use.  Here, it is asking the helper to use the AARF
	algorithm --- details are, of course, available in Doxygen.
	
Η μέθοδος ``SetRemoteStationManager`` λέει στον βοηθό τον τύπο του αλγορίθμου για τον
έλεγχο του ρυθμού που πρέπει να χρησιμοποιήσει. Εδώ, ζητάει από τον βοηθό να χρησιμοποιήσει τον
αλγόριθμο AARF --- λεπτομέρειες σχετικά με αυτόν υπάρχουν, φυσικά, στο Doxygen. 

..
	Next, we configure the type of MAC, the SSID of the infrastructure network we
	want to setup and make sure that our stations don't perform active probing:
	
Έπειτα, ρυθμίζουμε τον τύπο του MAC, το SSID του δικτύου υποδομής που θέλουμε να στήσουμε
και διασφαλίζουμε ότι οι σταθμοί μας δεν πραγματοποιούν ενεργητικές ανιχνεύσεις:

::

  Ssid ssid = Ssid ("ns-3-ssid");
  mac.SetType ("ns3::StaWifiMac",
    "Ssid", SsidValue (ssid),
    "ActiveProbing", BooleanValue (false));

..
	This code first creates an 802.11 service set identifier (SSID) object
	that will be used to set the value of the "Ssid" ``Attribute`` of
	the MAC layer implementation.  The particular kind of MAC layer that
	will be created by the helper is specified by ``Attribute`` as
	being of the "ns3::StaWifiMac" type.  The use of
	``NqosWifiMacHelper`` will ensure that the "QosSupported"
	``Attribute`` for created MAC objects is set false. The combination
	of these two configurations means that the MAC instance next created
	will be a non-QoS non-AP station (STA) in an infrastructure BSS (i.e.,
	a BSS with an AP).  Finally, the "ActiveProbing" ``Attribute`` is
	set to false.  This means that probe requests will not be sent by MACs
	created by this helper.
	
Αυτός ο κώδικας αρχικά δημιουργεί ένα αντικείμενο τύπου 802.11 SSID, το οποίο θα 
χρησιμοποιηθεί για να τεθεί η τιμή του "Ssid" ``Attribute`` της υλοποίησης του MAC
επιπέδου. Το συγκεκριμένο είδος επιπέδου MAC που θα δημιουργηθεί από τον βοηθό καθορίζεται 
μέσω ``Attribute`` ως τύπου "ns3::StaWifiMac". Η χρήση του ``NqosWifiMacHelper`` θα διασφαλίσει
ότι το ``Attribute`` "QosSupported" για τα δημιουργηθέντα αντικείμενα MAC θα τεθεί ως ψευδές. Ο
συνδυασμός αυτών των δύο ρυθμίσεων σημαίνει ότι το επόμενο αντικείμενο MAC που θα δημιουργηθεί
θα είναι ένας σταθμός (STA) non-QoS non-AP σε μια BSS υποδομή (π.χ. μία BSS με ένα AP). 
Τέλος, το ``Attribute`` "ActiveProbing" τίθεται ως ψευδές. Αυτό σημαίνει ότι δε θα στέλνονται
αιτήματα ανίχνευσης από τα MAC που δημιουργούνται από αυτόν τον βοηθό.

..
	Once all the station-specific parameters are fully configured, both at the
	MAC and PHY layers, we can invoke our now-familiar ``Install`` method to 
	create the wifi devices of these stations:
	
Μόλις ρυθμιστούν πλήρως όλες οι παράμετροι που αφορούν τους σταθμούς, τόσο στο MAC όσο και στο
φυσικό επίπεδο, μπορούμε να καλέσουμε τη γνωστή μας μέθοδο ``Install`` για να δημιουργήσουμε
τις συσκευές wifi σε αυτούς τους σταθμούς:

::

  NetDeviceContainer staDevices;
  staDevices = wifi.Install (phy, mac, wifiStaNodes);

..
	We have configured Wifi for all of our STA nodes, and now we need to 
	configure the AP (access point) node.  We begin this process by changing
	the default ``Attributes`` of the ``NqosWifiMacHelper`` to reflect the 
	requirements of the AP.
	
Έχουμε ρυθμίσει το Wifi για όλους τους STA κόμβους μας, και τώρα χρειάζεται να ρυθμίσουμε
τον AP κόμβο (σημείο πρόσβασης). Ξεκινάμε αυτή τη διαδικασία αλλάζοντας τα προεπιλεγμένα
``Attributes`` του ``NqosWifiMacHelper`` ώστε να ανταποκρίνονται στις απαιτήσεις του AP.

::

  mac.SetType ("ns3::ApWifiMac",
               "Ssid", SsidValue (ssid));

..
	In this case, the ``NqosWifiMacHelper`` is going to create MAC
	layers of the "ns3::ApWifiMac", the latter specifying that a MAC
	instance configured as an AP should be created, with the helper type
	implying that the "QosSupported" ``Attribute`` should be set to
	false - disabling 802.11e/WMM-style QoS support at created APs.  
	
Σε αυτή την περίπτωση, ο ``NqosWifiMacHelper`` θα δημιουργήσει επίπεδα MAC του 
"ns3::ApWifiMac", με το τελευταίο να ορίζει ότι πρέπει να δημιουργηθεί ένα MAC επίπεδο
ρυθμισμένο ως AP, με τον τύπο βοηθού να υποδηλώνει ότι το ``Attribute`` "QosSupported" πρέπει
να τεθεί ως ψευδές - απενεργοποιώντας την υποστήριξη τύπου 802.11e/WMM-style QoS στα AP που 
θα δημιουργηθούν.

..
	The next lines create the single AP which shares the same set of PHY-level
	``Attributes`` (and channel) as the stations:
	
Οι επόμενες γραμμές δημιουργούν ένα μοναδικό AP το οποίο μοιράζεται το ίδιο σύνολο από 
``Attributes`` φυσικού επιπέδου (και καναλιού) με τους σταθμούς:

::

  NetDeviceContainer apDevices;
  apDevices = wifi.Install (phy, mac, wifiApNode);

..
	Now, we are going to add mobility models.  We want the STA nodes to be mobile,
	wandering around inside a bounding box, and we want to make the AP node 
	stationary.  We use the ``MobilityHelper`` to make this easy for us.
	First, we instantiate a ``MobilityHelper`` object and set some 
	``Attributes`` controlling the "position allocator" functionality.
	
Σε αυτό το σημείο θα προσθέσουμε τα μοντέλα κινητικότητάς μας. Θέλουμε οι STA σταθμοί να είναι
κινητοί, περιπλανώμενοι μέσα στα πλαίσια ενός περιοριστικού κουτιού, και θέλουμε να κάνουμε τον
AP κόμβο σταθερό. Χρησιμοποιούμε τον ``MobilityHelper`` για να διευκολυνθούμε. Αρχικά, 
δημιουργούμε ένα αντικείμενο ``MobilityHelper`` και θέτουμε κάποια ``Attributes`` που ελέγχουν
τη λειτουργία του «κατανεμητή θέσεων» (position allocator).

::

  MobilityHelper mobility;

  mobility.SetPositionAllocator ("ns3::GridPositionAllocator",
    "MinX", DoubleValue (0.0),
    "MinY", DoubleValue (0.0),
    "DeltaX", DoubleValue (5.0),
    "DeltaY", DoubleValue (10.0),
    "GridWidth", UintegerValue (3),
    "LayoutType", StringValue ("RowFirst"));

..
	This code tells the mobility helper to use a two-dimensional grid to initially
	place the STA nodes.  Feel free to explore the Doxygen for class 
	``ns3::GridPositionAllocator`` to see exactly what is being done.
	
Αυτός ο κώδικας λέει στον βοηθό κινητικότητας να χρησιμοποιήσει ένα δισδιάστατο πλέγμα για 
να τοποθετήσει αρχικά τους STA κόμβους. Εξερευνήστε ελεύθερα το Doxygen ψάχνοντας για την κλάση
``ns3::GridPositionAllocator`` για να δείτε ακριβώς τι γίνεται.

..
	We have arranged our nodes on an initial grid, but now we need to tell them
	how to move.  We choose the ``RandomWalk2dMobilityModel`` which has the 
	nodes move in a random direction at a random speed around inside a bounding 
	box.
	
Έχουμε τοποθετήσει τους κόμβους μας στο αρχικό πλέγμα, αλλά τώρα πρέπει να τους πούμε πώς να 
κινηθούν. Επιλέγουμε το ``RandomWalk2dMobilityModel``, το οποίο βάζει τους κόμβους να κινούνται προς
μια τυχαία κατεύθυνση, με τυχαία ταχύτητα, μέσα σε ένα οριοθετημένο κουτί.

::

  mobility.SetMobilityModel ("ns3::RandomWalk2dMobilityModel",
    "Bounds", RectangleValue (Rectangle (-50, 50, -50, 50)));

..
	We now tell the ``MobilityHelper`` to install the mobility models on the 
	STA nodes.
	
Τώρα λέμε στο ``MobilityHelper`` να εγκαταστήσει τα μοντέλα κινητικότητας στους STA κόμβους.

::

  mobility.Install (wifiStaNodes);

..
	We want the access point to remain in a fixed position during the simulation.
	We accomplish this by setting the mobility model for this node to be the 
	``ns3::ConstantPositionMobilityModel``:
	
Θέλουμε το σημείο πρόσβασης να παραμείνει σε μια καθορισμένη θέση κατά τη διάρκεια της
προσομοίωσης. Αυτό το επιτυγχάνουμε θέτοντας ως μοντέλο κινητικότητας για αυτόν τον κόμβο
το ``ns3::ConstantPositionMobilityModel``:

::

  mobility.SetMobilityModel ("ns3::ConstantPositionMobilityModel");
  mobility.Install (wifiApNode);

..
	We now have our nodes, devices and channels created, and mobility models 
	chosen for the Wifi nodes, but we have no protocol stacks present.  Just as 
	we have done previously many times, we will use the ``InternetStackHelper``
	to install these stacks.
	
Τώρα πλέον έχουμε δημιουργήσει τους κόμβους μας, τις συσκευές και τα κανάλια μας, έχουμε
επιλέξει τα μοντέλα κινητικότητας για τους Wifi κόμβους, αλλά δεν έχουμε καμία στοίβα πρωτοκόλλου.
Ακριβώς όπως κάναμε και πολλές φορές πριν, θα χρησιμοποιήσουμε τον ``InternetStackHelper`` για να
εγκαταστήσουμε αυτές τις στοίβες.

::

  InternetStackHelper stack;
  stack.Install (csmaNodes);
  stack.Install (wifiApNode);
  stack.Install (wifiStaNodes);

..
	Just as in the ``second.cc`` example script, we are going to use the 
	``Ipv4AddressHelper`` to assign IP addresses to our device interfaces.
	First we use the network 10.1.1.0 to create the two addresses needed for our
	two point-to-point devices.  Then we use network 10.1.2.0 to assign addresses
	to the CSMA network and then we assign addresses from network 10.1.3.0 to
	both the STA devices and the AP on the wireless network.
	
Όπως και στο παράδειγμα του ``second.cc``, θα χρησιμοποιήσουμε τον ``Ipv4AddressHelper``
για να αναθέσουμε IP διευθύνσεις στις διεπαφές των συσκευών μας. Αρχικά θα χρησιμοποιήσουμε 
το δίκτυο 10.1.1.0 για να δημιουργήσουμε τις δύο διευθύνσεις που χρειαζονται οι δύο συσκευές μας
σημείου-προς-σημείο. Έπειτα χρησιμοποιούμε το δίκτυο 10.1.2.0 για να αναθέσουμε διευθύνσεις στο
CSMA δίκτυο, και έπειτα αναθέτουμε διευθύνσεις από το δίκτυο 10.1.3.0 τόσο στις STA συσκευές όσο και
στην AP συσκευή στο ασύρματο δίκτυο.

::

  Ipv4AddressHelper address;

  address.SetBase ("10.1.1.0", "255.255.255.0");
  Ipv4InterfaceContainer p2pInterfaces;
  p2pInterfaces = address.Assign (p2pDevices);

  address.SetBase ("10.1.2.0", "255.255.255.0");
  Ipv4InterfaceContainer csmaInterfaces;
  csmaInterfaces = address.Assign (csmaDevices);

  address.SetBase ("10.1.3.0", "255.255.255.0");
  address.Assign (staDevices);
  address.Assign (apDevices);

..
	We put the echo server on the "rightmost" node in the illustration at the
	start of the file.  We have done this before.
	
Τοποθετούμε τον εξυπηρετητή echo στον «δεξιότερο» κόμβο της απεικόνισης στην αρχή του αρχείου.
Το έχουμε κάνει και πιο πριν αυτό.

::

  UdpEchoServerHelper echoServer (9);

  ApplicationContainer serverApps = echoServer.Install (csmaNodes.Get (nCsma));
  serverApps.Start (Seconds (1.0));
  serverApps.Stop (Seconds (10.0));

..
	And we put the echo client on the last STA node we created, pointing it to
	the server on the CSMA network.  We have also seen similar operations before.
	
Τοποθετούμε και τον πελάτη echo στον τελευταίο STA κόμβο που δημιουργήσαμε, κατευθύνοντάς τον
προς τον εξυπηρετητή του CSMA δικτύου. Έχουμε επίσης δει παρόμοιες λειτουργίες και παλιότερα.

::

  UdpEchoClientHelper echoClient (csmaInterfaces.GetAddress (nCsma), 9);
  echoClient.SetAttribute ("MaxPackets", UintegerValue (1));
  echoClient.SetAttribute ("Interval", TimeValue (Seconds (1.0)));
  echoClient.SetAttribute ("PacketSize", UintegerValue (1024));

  ApplicationContainer clientApps =
    echoClient.Install (wifiStaNodes.Get (nWifi - 1));
  clientApps.Start (Seconds (2.0));
  clientApps.Stop (Seconds (10.0));

..
	Since we have built an internetwork here, we need to enable internetwork routing
	just as we did in the ``second.cc`` example script.
	
Αφότου έχουμε χτίσει ένα διαδίκτυο εδώ, χρειάζεται να ενεργοποιήσουμε τη δρομολόγηση διαδικτύου
όπως κάναμε και στο σενάριο του παραδείγματος στο ``second.cc``.

::

  Ipv4GlobalRoutingHelper::PopulateRoutingTables ();

..
	One thing that can surprise some users is the fact that the simulation we just
	created will never "naturally" stop.  This is because we asked the wireless
	access point to generate beacons.  It will generate beacons forever, and this
	will result in simulator events being scheduled into the future indefinitely,
	so we must tell the simulator to stop even though it may have beacon generation
	events scheduled.  The following line of code tells the simulator to stop so that 
	we don't simulate beacons forever and enter what is essentially an endless
	loop.
	
Ένα πράγμα που μπορεί να εκπλήσσει κάποιους χρήστες είναι το γεγονός ότι η προσομοίωση που
μόλις δημιουργήσαμε δε θα σταματήσει ποτέ «εκ των πραγμάτων». Αυτό οφείλεται στο ότι
ζητήσαμε από το ασύρματο σημείο πρόσβασης να παράγει beacons. Θα παράγει beacons για πάντα,
και αυτό θα έχει ως αποτέλεσμα να προγραμματίζονται ασταμάτητα γεγονότα προσομοίωσης για το μέλλον,
οπότε πρέπει να πούμε στον προσομοιωτή να σταματήσει, παρόλο που μπορεί να έχουν προγραμματιστεί
γεγονότα δημιουργίας beacon. Η ακόλουθη γραμμή κώδικα λέει στον προσομοιωτή να σταματήσει έτσι ώστε
να μην προσομοιώνουμε beacons για πάντα, μπαίνοντας με αυτόν τον τρόπο σε έναν κατ' ουσίαν 
ατέρμονα βρόχο.

::

  Simulator::Stop (Seconds (10.0));

.. We create just enough tracing to cover all three networks:

Δημιουργούμε τόσα ίχνη καταγραφής έτσι ώστε να καλύψουμε και τα τρία δίκτυα:

::

  pointToPoint.EnablePcapAll ("third");
  phy.EnablePcap ("third", apDevices.Get (0));
  csma.EnablePcap ("third", csmaDevices.Get (0), true);

..
	These three lines of code will start pcap tracing on both of the point-to-point
	nodes that serves as our backbone, will start a promiscuous (monitor) mode 
	trace on the Wifi network, and will start a promiscuous trace on the CSMA 
	network.  This will let us see all of the traffic with a minimum number of 
	trace files.
	
Αυτές οι τρεις γραμμές κώδικα θα ξεκινήσουν την καταγραφή pcap και στους δύο κόμβους 
σημείου-προς-σημείο που λειτουργούν ως η ραχοκοκαλιά μας, θα αρχίσουν μια καταγραφή 
μεικτής κατάστασης στο Wifi δίκτυο, και μια μεικτή καταγραφή στο CSMA δίκτυο. Αυτό θα 
μας επιτρέψει να δούμε όλη την κίνηση με τη βοήθεια του ελάχιστου αριθμού αρχείων ιχνών.  

.. Finally, we actually run the simulation, clean up and then exit the program.

Τέλος, τρέχουμε όντως την προσομοίωση, καθαρίζουμε και έπειτα βγαίνουμε από το πρόγραμμα.

::

    Simulator::Run ();
    Simulator::Destroy ();
    return 0;
  }

..
	In order to run this example, you have to copy the ``third.cc`` example
	script into the scratch directory and use Waf to build just as you did with
	the ``second.cc`` example.  If you are in the top-level directory of the
	repository you would type,
	
Για να τρέξετε αυτό το παράδειγμα, θα πρέπει να αντιγράψετε το σενάριο ``third.cc`` στον
κατάλογο scratch και να χρησιμοποιήσετε το Waf για να κάνετε build, όπως κάνατε και στο
παράδειγμα ``second.cc``. Εάν είστε στον κατάλογο του υψηλότερου επιπέδου του αποθετηρίου,
θα πληκτρολογήσετε,

.. sourcecode:: bash

  $ cp examples/tutorial/third.cc scratch/mythird.cc
  $ ./waf
  $ ./waf --run scratch/mythird

..
	Again, since we have set up the UDP echo applications just as we did in the 
	``second.cc`` script, you will see similar output.
	
Ξανά, από τη στιγμή που έχετε θέσει τις εφαρμογές UDP echo όπως κάναμε στο σενάριο
``second.cc``, θα δείτε μία παρόμοια έξοδο.

.. sourcecode:: text

  Waf: Entering directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  Waf: Leaving directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  'build' finished successfully (0.407s)
  At time 2s client sent 1024 bytes to 10.1.2.4 port 9
  At time 2.01796s server received 1024 bytes from 10.1.3.3 port 49153
  At time 2.01796s server sent 1024 bytes to 10.1.3.3 port 49153
  At time 2.03364s client received 1024 bytes from 10.1.2.4 port 9

..
	Recall that the first message, ``Sent 1024 bytes to 10.1.2.4``," is the 
	UDP echo client sending a packet to the server.  In this case, the client
	is on the wireless network (10.1.3.0).  The second message, 
	"``Received 1024 bytes from 10.1.3.3``," is from the UDP echo server, 
	generated when it receives the echo packet.  The final message, 
	"``Received 1024 bytes from 10.1.2.4``," is from the echo client, indicating
	that it has received its echo back from the server.
	
Θυμηθείτε ότι το πρώτο μήνυμα, "``Sent 1024 bytes to 10.1.2.4``", είναι ο πελάτης 
UDP echo που στέλνει ένα πακέτο στον εξυπηρετητή. Σε αυτή την περίπτωση, ο πελάτης είναι
στο ασύρματο δίκτυο (10.1.3.0). Το δεύτερο μήνυμα, "``Received 1024 bytes from 10.1.3.3``", 
είναι από τον UDP echo εξυπηρετητή, και δημιουργήθηκε όταν αυτός έλαβε το echo πακέτο. Το τελικό
μήνυμα, "``Received 1024 bytes from 10.1.2.4``", είναι από τον πελάτη echo, και δείχνει ότι
αυτός έχει λάβει το echo πακέτο του πίσω από τον εξυπηρετητή.

..
	If you now go and look in the top level directory, you will find four trace 
	files from this simulation, two from node zero and two from node one:
	
Εάν τώρα πάτε και δείτε στον κατάλογο του υψηλότερου επιπέδου, θα βρείτε τέσσερα αρχεία
ιχνών από την προσομοίωση, δύο από τον κόμβο μηδέν και δύο από τον κόμβο ένα: 

.. sourcecode:: text

  third-0-0.pcap  third-0-1.pcap  third-1-0.pcap  third-1-1.pcap

..
	The file "third-0-0.pcap" corresponds to the point-to-point device on node
	zero -- the left side of the "backbone".  The file "third-1-0.pcap" 
	corresponds to the point-to-point device on node one -- the right side of the
	"backbone".  The file "third-0-1.pcap" will be the promiscuous (monitor
	mode) trace from the Wifi network and the file "third-1-1.pcap" will be the
	promiscuous trace from the CSMA network.  Can you verify this by inspecting
	the code?
	
Το αρχείο "third-0-0.pcap" αντιστοιχεί στη συσκευή σημείου-προς-σημείο στον κόμβο μηδέν --
στην αριστερή πλευρά της «ραχοκοκαλιάς». Το αρχείο "third-1-0.pcap" αντιστοιχεί στη συσκευή
σημείου-προς-σημείο στον κόμβο ένα -- στην δεξιά πλευρά της "ραχοκοκαλιάς". Το αρχείο 
"third-0-1.pcap" θα είναι το μεικτό (κατάσταση παρακολούθησης) ίχνος από το Wifi δίκτυο και το
αρχείο "third-1-1.pcap" θα είναι το μεικτό ίχνος από το CSMA δίκτυο. Μπορείτε να το επιβεβαιώσετε
αυτό εξετάζοντας τον κώδικα;

..
	Since the echo client is on the Wifi network, let's start there.  Let's take
	a look at the promiscuous (monitor mode) trace we captured on that network.
	
Από τη στιγμή που ο echo πελάτης είναι στο Wifi δίκτυο, ας αρχίσουμε από εκεί. Ας δούμε
στο μεικτό (σε κατάσταση παρακολούθησης) ίχνος που καταγράψαμε σε αυτό το δίκτυο.

.. sourcecode:: bash

  $ tcpdump -nn -tt -r third-0-1.pcap

.. You should see some wifi-looking contents you haven't seen here before:

Θα πρέπει να δείτε κάποια περιεχόμενα σχετικά με το Wifi που δεν έχετε ξαναδεί προηγουμένως:

.. sourcecode:: text

  reading from file third-0-1.pcap, link-type IEEE802_11 (802.11)
  0.000025 Beacon (ns-3-ssid) [6.0* 9.0 12.0 18.0 24.0 36.0 48.0 54.0 Mbit] IBSS
  0.000308 Assoc Request (ns-3-ssid) [6.0 9.0 12.0 18.0 24.0 36.0 48.0 54.0 Mbit]
  0.000324 Acknowledgment RA:00:00:00:00:00:08 
  0.000402 Assoc Response AID(0) :: Successful
  0.000546 Acknowledgment RA:00:00:00:00:00:0a 
  0.000721 Assoc Request (ns-3-ssid) [6.0 9.0 12.0 18.0 24.0 36.0 48.0 54.0 Mbit]
  0.000737 Acknowledgment RA:00:00:00:00:00:07 
  0.000824 Assoc Response AID(0) :: Successful
  0.000968 Acknowledgment RA:00:00:00:00:00:0a 
  0.001134 Assoc Request (ns-3-ssid) [6.0 9.0 12.0 18.0 24.0 36.0 48.0 54.0 Mbit]
  0.001150 Acknowledgment RA:00:00:00:00:00:09 
  0.001273 Assoc Response AID(0) :: Successful
  0.001417 Acknowledgment RA:00:00:00:00:00:0a 
  0.102400 Beacon (ns-3-ssid) [6.0* 9.0 12.0 18.0 24.0 36.0 48.0 54.0 Mbit] IBSS
  0.204800 Beacon (ns-3-ssid) [6.0* 9.0 12.0 18.0 24.0 36.0 48.0 54.0 Mbit] IBSS
  0.307200 Beacon (ns-3-ssid) [6.0* 9.0 12.0 18.0 24.0 36.0 48.0 54.0 Mbit] IBSS

..
	You can see that the link type is now 802.11 as you would expect.  You can 
	probably understand what is going on and find the IP echo request and response
	packets in this trace.  We leave it as an exercise to completely parse the 
	trace dump.
	
Μπορείτε να δείτε ότι ο τύπος σύνδεσης είναι τώρα ο 802.11, όπως θα περιμένατε. Μπορείτε
πιθανώς να καταλάβετε τι γίνεται και να βρείτε τα πακέτα του IP echo αιτήματος και της απάντησης
σε αυτό το ίχνος. Την πλήρη ανάλυση των ιχνών αυτών σας την αφήνουμε ως άσκηση.

.. Now, look at the pcap file of the right side of the point-to-point link,

Τώρα, δείτε στο αρχείο pcap της αριστερής πλευράς του συνδέσμου σημείου-προς-σημείο,

.. sourcecode:: bash

  $ tcpdump -nn -tt -r third-0-0.pcap

.. Again, you should see some familiar looking contents:

Ξανά, θα δείτε μερικά γνώριμα περιεχόμενα: 

.. sourcecode:: text

  reading from file third-0-0.pcap, link-type PPP (PPP)
  2.008151 IP 10.1.3.3.49153 > 10.1.2.4.9: UDP, length 1024
  2.026758 IP 10.1.2.4.9 > 10.1.3.3.49153: UDP, length 1024

.. 
	This is the echo packet going from left to right (from Wifi to CSMA) and back
	again across the point-to-point link.
	
Αυτό είναι το echo πακέτο που πηγαίνει από αριστερά προς τα δεξιά (από το Wifi στο CSMA) και 
ξανά πίσω διαμέσου του συνδέσμου σημείου-προς-σημείο.

.. Now, look at the pcap file of the right side of the point-to-point link,

Τώρα, δείτε στο αρχείο pcap της δεξιάς πλευράς του συνδέσμου σημείου-προς-σημείο,

.. sourcecode:: bash

  $ tcpdump -nn -tt -r third-1-0.pcap

.. Again, you should see some familiar looking contents:

Ξανά, θα δείτε μερικά γνώριμα περιεχόμενα: 

.. sourcecode:: text

  reading from file third-1-0.pcap, link-type PPP (PPP)
  2.011837 IP 10.1.3.3.49153 > 10.1.2.4.9: UDP, length 1024
  2.023072 IP 10.1.2.4.9 > 10.1.3.3.49153: UDP, length 1024

.. 
	This is also the echo packet going from left to right (from Wifi to CSMA) and 
	back again across the point-to-point link with slightly different timings
	as you might expect.
	
Αυτό είναι επίσης το echo πακέτο που πηγαίνει από τα αριστερά προς τα δεξιά (από το Wifi στο 
CSMA) και πάλι πίσω διαμέσου του συνδέσμου σημείου-προς-σημείο, με λίγο διαφορετικούς χρονισμούς
όπως πιθανόν να αναμένατε.

..
	The echo server is on the CSMA network, let's look at the promiscuous trace 
	there:
	
Ο echo εξυπηρετητής βρίσκεται στο CSMA δίκτυο, οπότε ας ρίξουμε μια ματιά στο μεικτό ίχνος εκεί:

.. sourcecode:: bash

  $ tcpdump -nn -tt -r third-1-1.pcap

.. You should see some familiar looking contents:

Θα πρέπει να βλέπετε μερικά γνώριμα περιεχόμενα:

.. sourcecode:: text

  reading from file third-1-1.pcap, link-type EN10MB (Ethernet)
  2.017837 ARP, Request who-has 10.1.2.4 (ff:ff:ff:ff:ff:ff) tell 10.1.2.1, length 50
  2.017861 ARP, Reply 10.1.2.4 is-at 00:00:00:00:00:06, length 50
  2.017861 IP 10.1.3.3.49153 > 10.1.2.4.9: UDP, length 1024
  2.022966 ARP, Request who-has 10.1.2.1 (ff:ff:ff:ff:ff:ff) tell 10.1.2.4, length 50
  2.022966 ARP, Reply 10.1.2.1 is-at 00:00:00:00:00:03, length 50
  2.023072 IP 10.1.2.4.9 > 10.1.3.3.49153: UDP, length 1024

..
	This should be easily understood.  If you've forgotten, go back and look at
	the discussion in ``second.cc``.  This is the same sequence.
	
Αυτό θα πρέπει να είναι εύκολα κατανοητό. Εάν έχετε ξεχάσει τι και πώς, επιστρέψτε πίσω και κοιτάξτε
στα όσα είπαμε στο παράδειγμα ``second.cc``. Είναι η ίδια ακολουθία.

..
	Now, we spent a lot of time setting up mobility models for the wireless network
	and so it would be a shame to finish up without even showing that the STA
	nodes are actually moving around during the simulation.  Let's do this by hooking
	into the ``MobilityModel`` course change trace source.  This is just a sneak
	peek into the detailed tracing section which is coming up, but this seems a very
	nice place to get an example in.
	
Τώρα, αφιερώσαμε αρκετό χρόνο καθορίζοντας τα μοντέλα κινητικότητας για το ασύρματο δίκτυο
και έτσι θα ήταν κρίμα να κλείσουμε χωρίς καν να δείξουμε ότι όντως οι STA κόμβοι κινούνται
κατά τη διάρκεια της προσομοίωσης. Ας το κάνουμε αυτό εξετάζοντας τον πηγαίο κώδικα της καταγραφής
των αλλαγών πορείας του ``MobilityModel``. Πρόκειται απλά για μια γρήγορη ματιά στην λεπτομερή
ενότητα σχετικά με την ιχνηλασία που ακολουθεί μετέπειτα, μα το σημείο αυτό φαίνεται ιδανικό
για να δούμε ένα σχετικό παράδειγμα.

..
	As mentioned in the "Tweaking ns-3" section, the |ns3| tracing system 
	is divided into trace sources and trace sinks, and we provide functions to 
	connect the two.  We will use the mobility model predefined course change 
	trace source to originate the trace events.  We will need to write a trace 
	sink to connect to that source that will display some pretty information for 
	us.  Despite its reputation as being difficult, it's really quite simple.
	Just before the main program of the ``scratch/mythird.cc`` script (i.e.,
	just after the ``NS_LOG_COMPONENT_DEFINE`` statement), add the 
	following function:
	
Όπως αναφέρθηκε στην ενότητα «Μικρορυθμίσεις», το σύστημα ιχνηλασίας του |ns3| 
χωρίζεται σε πηγές ιχνηλασίας (trace source) και καταβόθρες ιχνηλασίας (trace sinks), και 
εμείς παρέχουμε μεθόδους για τη σύνδεση αυτών των δύο. Θα χρησιμοποιήσουμε την προκαθορισμένη
πηγή ιχνηλασίας των αλλαγών πορείας για το μοντέλο κινητικότητας ώστε να πυροδοτήσουμε τα 
γεγονότα ιχνηλασίας. Θα χρειαστεί να συντάξουμε μια καταβόθρα ιχνηλασίας ώστε να τη συνδέσουμε 
με την πηγή, η οποία θα μας εμφανίζει μερικές ωραίες πληροφορίες. Παρά τη φήμη περί δυσκολίας αυτού
του πράγματος, στην πραγματικότητα είναι εξαιρετικά απλό. Απλά πριν το κυρίως πρόγραμμα του
σεναρίου ``scratch/mythird.cc`` (π.χ. αμέσως μετά τη δήλωση ``NS_LOG_COMPONENT_DEFINE``),
προσθέστε την ακόλουθη συνάρτηση: 

::

  void
  CourseChange (std::string context, Ptr<const MobilityModel> model)
  {
    Vector position = model->GetPosition ();
    NS_LOG_UNCOND (context << 
      " x = " << position.x << ", y = " << position.y);
  }

..
	This code just pulls the position information from the mobility model and 
	unconditionally logs the x and y position of the node.  We are
	going to arrange for this function to be called every time the wireless
	node with the echo client changes its position.  We do this using the 
	``Config::Connect`` function.  Add the following lines of code to the
	script just before the ``Simulator::Run`` call.
	
Αυτός ο κώδικας τραβάει τις πληροφορίες τοποθεσίας από το μοντέλο κινητικότητας και 
καταγράφει άνευ όρων τις συντεταγμένες x και y του κόμβου. Θα τα ρυθμίσουμε έτσι ώστε αυτή η μέθοδος
να καλείται κάθε φορά που ο ασύρματος κόμβος με τον echo πελάτη αλλάζει θέση. Αυτό το πετυχαίνουμε
με τη χρήση της συνάρτησης ``Config::Connect``. Προσθέστε τις ακόλουθες γραμμές κώδικα στο 
σενάριο, ακριβώς πριν από την κλήση ``Simulator::Run``.

::

  std::ostringstream oss;
  oss <<
    "/NodeList/" << wifiStaNodes.Get (nWifi - 1)->GetId () <<
    "/$ns3::MobilityModel/CourseChange";

  Config::Connect (oss.str (), MakeCallback (&CourseChange));

..
	What we do here is to create a string containing the tracing namespace path
	of the event to which we want to connect.  First, we have to figure out which 
	node it is we want using the ``GetId`` method as described earlier.  In the
	case of the default number of CSMA and wireless nodes, this turns out to be 
	node seven and the tracing namespace path to the mobility model would look
	like,
	
Αυτό που κάνουμε εδώ είναι ότι δημιουργούμε μια ακολουθία που περιέχει το μονοπάτι του χώρου
ονομάτων που αφορά στην ιχνηλασία του γεγονότος στο οποίο θέλουμε να συνδεθούμε. Αρχικά, πρέπει
να καταλάβουμε ποιος είναι ο κόμβος που θέλουμε, χρησιμοποιώντας τη μέθοδο ``GetId`` όπως
περιγράφθηκε νωρίτερα. Στην περίπτωση του εξ ορισμού αριθμού CSMA και ασύρματων κόμβων, προκύπτει
ότι ο ζητούμενος κόμβος είναι ο κόμβος επτά και το μονοπάτι του χώρου ονομάτων που αφορά
στην ιχνηλασία για το μοντέλο κινητικότητας θα μοιάζει κάπως έτσι:

::

  /NodeList/7/$ns3::MobilityModel/CourseChange

..
	Based on the discussion in the tracing section, you may infer that this trace 
	path references the seventh node in the global NodeList.  It specifies
	what is called an aggregated object of type ``ns3::MobilityModel``.  The 
	dollar sign prefix implies that the MobilityModel is aggregated to node seven.
	The last component of the path means that we are hooking into the 
	"CourseChange" event of that model. 
	
Με βάση τα όσα είπαμε στην ενότητα περί ιχνηλασίας, μπορείτε να παρέμβετε έτσι ώστε το 
μονοπάτι ιχνηλασίας να αναφέρεται στον έβδομο κόμβο της καθολικής NodeList. Αυτό ορίζει
κάτι το οποίο αποκαλείται ενσωματωμένο (aggregated) αντικείμενο τύπου ``ns3::MobilityModel``. Το πρόθεμα 
του δολαρίου υποδηλώνει ότι το MobilityModel είναι ενσωματωμένο στον κόμβο επτά. Το τελευταιό
μέρος του μονοπατιού σημαίνει ότι βρισκόμαστε στο γεγονός "CourseChange" αυτού του μοντέλου.

..
	We make a connection between the trace source in node seven with our trace 
	sink by calling ``Config::Connect`` and passing this namespace path.  Once 
	this is done, every course change event on node seven will be hooked into our 
	trace sink, which will in turn print out the new position.
	
Κάνουμε τη σύνδεση μεταξύ της πηγής ιχνηλασίας στον κόμβο επτά με την καταβόθρα ιχνηλασίας μας,
καλώντας την ``Config::Connect`` και περνώντας ως όρισμα το μονοπάτι του χώρου ονομάτων. Μόλις 
γίνει αυτό, κάθε γεγονός αλλαγής πορείας στον κόμβο επτά θα καταλήγει στην καταβόθρα ιχνηλασίας
μας, η οποία με τη σειρά της θα εκτυπώνει τη νέα θέση.

..
	If you now run the simulation, you will see the course changes displayed as 
	they happen.

Εάν τώρα τρέξετε την προσομοίωση, θα δείτε ότι οι αλλαγές πορείας εμφανίζονται καθώς συμβαίνουν.

.. sourcecode:: text

  'build' finished successfully (5.989s)
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 10, y = 0
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 10.3841, y = 0.923277
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 10.2049, y = 1.90708
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 10.8136, y = 1.11368
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 10.8452, y = 2.11318
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 10.9797, y = 3.10409
  At time 2s client sent 1024 bytes to 10.1.2.4 port 9
  At time 2.01796s server received 1024 bytes from 10.1.3.3 port 49153
  At time 2.01796s server sent 1024 bytes to 10.1.3.3 port 49153
  At time 2.03364s client received 1024 bytes from 10.1.2.4 port 9
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 11.3273, y = 4.04175
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 12.013, y = 4.76955
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 12.4317, y = 5.67771
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 11.4607, y = 5.91681
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 12.0155, y = 6.74878
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 13.0076, y = 6.62336
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 12.6285, y = 5.698
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 13.32, y = 4.97559
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 13.1134, y = 3.99715
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 13.8359, y = 4.68851
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 13.5953, y = 3.71789
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 12.7595, y = 4.26688
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 11.7629, y = 4.34913
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 11.2292, y = 5.19485
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 10.2344, y = 5.09394
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 9.3601, y = 4.60846
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 8.40025, y = 4.32795
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 9.14292, y = 4.99761
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 9.08299, y = 5.99581
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 8.26068, y = 5.42677
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 8.35917, y = 6.42191
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 7.66805, y = 7.14466
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 6.71414, y = 6.84456
  /NodeList/7/$ns3::MobilityModel/CourseChange x = 6.42489, y = 7.80181
