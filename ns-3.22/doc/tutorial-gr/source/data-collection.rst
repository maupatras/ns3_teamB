.. include:: replace.txt
.. highlight:: cpp
.. role:: raw-role(raw)
   :format: html latex

..
	========================================================================================
	Current translation in Greek was done as a project of a ns3-related seminar organized by
	Greek Free/Open Source Software Society (ellak.gr) at University of Patras in 2014. The
	members of the translation team were:
		* Costas Deltouzos (costas.deltouzos@gmail.com)
		* Vasileios Dimitropoulos (vasdimitrop@upatras.gr)
		* Giorgos Kaffezas (kaffezas@ceid.upatras.gr)
		
	The team that is responsible for keeping the translation up-to-date consists of:
		# [name_of_team_member_1]
		# [name_of_team_member_2]
		# [name_of_team_member_3]
	----------------------------------------------------------------------------------------
	> Current file was initially translated by G. Kaffezas.
	> Last update was performed at 2015-04-27 by G. Kaffezas.
	========================================================================================


.. Data Collection

Συλλογή Δεδομένων
-----------------

..
	Our final tutorial chapter introduces some components that were added
	to |ns3| in version 3.18, and that are still under development.  This
	tutorial section is also a work-in-progress.

Το τελευταίο κεφάλαιο του οδηγού μας παρουσιάζει κάποια συστατικά μέρη που προστέθηκαν
στον |ns3| κατά την έκδοση 3.18, και τα οποία είναι ακόμα υπό ανάπτυξη. Όπως επίσης
είναι υπό ανάπτυξη και αυτό το μέρος του οδηγού.

.. Motivation

Κίνηση
******

..
	One of the main points of running simulations is to generate output data, 
	either for research purposes or simply to learn about the system.
	In the previous chapter, we introduced the tracing subsystem and
	the example ``sixth.cc``. from which PCAP or ASCII trace files are 
	generated.  These traces are valuable for data analysis using a
	variety of external tools, and for many users, such output data is
	a preferred means of gathering data (for analysis by external tools).

Ένας από τους κύριους στόχους της εκτέλεσης προσομοιώσεων είναι η δημιουργία δεδομένων
εξόδου, είτε για ερευνητικούς σκοπούς είτε απλά για την εκμάθηση του συστήματος.
Στο προηγούμενο κεφάλαιο, εισαγάγαμε το υποσύστημα ιχνηλασίας (tracing) και το παράδειγμα
``sixth.cc`` από το οποίο παράγονται PCAP ή ASCII αρχεία ιχνών. Αυτά τα ίχνη
είναι πολύτιμα για την ανάλυση δεδομένων με χρήση ποικιλίας εξωτερικών εργαλείων, και
για πολλούς χρήστες, καθώς τα δεδομένα εξόδου είναι ένα μέσο που προτιμάται για
τη συλλογή δεδομένων (για ανάλυση από εξωτερικά εργαλεία).

..
	However, there are also use cases for more than trace file generation,
	including the following:
	
Ωστόσο, υπάρχουν επίσης περιπτώσεις χρήσης που έχουν να κάνουν με περισσότερα
από την απλή δημιουργία αρχείων ιχνηλασίας, συμπεριλαμβανομένων και των ακόλουθων:

..
	* generation of data that does not map well to PCAP or ASCII traces, such
	  as non-packet data (e.g. protocol state machine transitions),
	* large simulations for which the disk I/O requirements for generating
	  trace files is prohibitive or cumbersome, and
	* the need for *online*  data reduction or computation, during the course
	  of the simulation.  A good example of this is to define a termination
	  condition for the simulation, to tell it when to stop when it has 
	  received enough data to form a narrow-enough confidence interval around
	  the estimate of some parameter.
	  
* δημιουργία δεδομένων, που δεν καταγράφονται καλά σε ίχνη PCAP ή ASCII, όπως
  είναι τα δεδομένα εκτός των πακέτων (π.χ. μεταβάσεις καταστάσεων μηχανής σύμφωνα
  με πρωτόκολλα)
* μεγάλες προσομοιώσεις, για τις οποίες οι απαιτήσεις εισόδου-εξόδου σε χωρητικότητα
  για τη δημιουργία αρχείων ιχνηλασίας είναι απαγορευτικές ή επιβαρυντικές, και
* η ανάγκη για αναγωγή δεδομένων ή υπολογισμό *σε πραγματικό χρόνο* (online), κατά τη διάρκεια της
  εκτέλεσης της προσομοίωσης. Ένα καλό παράδειγμα σχετικά με αυτό είναι ο ορισμός
  μιας τερματικής συνθήκης για την προσομοίωση, ώστε να καθορίσετε το πότε να σταματήσει,
  όταν έχει λάβει αρκετά δεδομένα ώστε να σχηματίσει ένα αρκετά περιορισμένο διάστημα 
  εμπιστοσύνης γύρω από την εκτίμηση κάποιας παραμέτρου.

..
	The |ns3| data collection framework is designed to provide these
	additional capabilities beyond trace-based output.  We recommend
	that the reader interested in this topic consult the |ns3| Manual
	for a more detailed treatment of this framework; here, we summarize
	with an example program some of the developing capabilities. 
	
Το πλαίσιο συλλογής δεδομένων του |ns3| έχει σχεδιαστεί ώστε να παρέχει αυτές τις
επιπρόσθετες δυνατότητες πέρα από αποτελέσματα που βασίζονται σε ίχνη. Συνιστούμε
στους αναγνώστες που ενδιαφέρονται για αυτό το θέμα να συμβουλευτούν το εγχειρίδιο
του |ns3| για μια πιο λεπτομερή εξέταση του πλαισίου αυτού. Για τώρα, συνοψίζουμε
μέσω ενός παραδείγματος κάποιες από τις δυνατότητες ανάπτυξης.

.. Example Code

Παράδειγμα
**********

..
	The tutorial example ``examples/tutorial/seventh.cc`` resembles the
	``sixth.cc`` example we previously reviewed, except for a few changes.
	First, it has been enabled for IPv6 support with a command-line option:
	
Το παράδειγμα του οδηγού ``examples/tutorial/seventh.cc`` μοιάζει με το 
παράδειγμα ``sixth.cc`` που εξετάσαμε προηγουμένως, πέρα από κάποιες αλλαγές.
Αρχικά, έχει ενεργοποιηθεί η υποστήριξη για IPv6 με μια επιλογή μέσω τερματικού:

::   

  CommandLine cmd;
  cmd.AddValue ("useIpv6", "Use Ipv6", useV6);
  cmd.Parse (argc, argv);

..
	If the user specifies ``useIpv6``, option, the program will be run
	using IPv6 instead of IPv4.  The ``help`` option, available on all |ns3| 
	programs that support the CommandLine object as shown above, can
	be invoked as follows (please note the use of double quotes):
	
Αν ο χρήστης κάνει την επιλογή ``useIpv6``, το πρόγραμμα θα εκτελεστεί χρησιμοποιώντας
το IPv6 αντί του IPv4. Η επιλογή ``help``, διαθέσιμη σε όλα τα προγράμματα του |ns3|
που υποστηρίζουν το αντικείμενο CommandLine όπως φαίνεται παραπάνω, μπορεί να 
καλεστεί ως ακολούθως (παρακαλούμε προσέξτε τη χρήση των διπλών εισαγωγικών):

::

  ./waf --run "seventh --help"

.. which produces:

η οποία παράγει:

::

  ns3-dev-seventh-debug [Program Arguments] [General Arguments]
  
  Program Arguments:
      --useIpv6:  Use Ipv6 [false]
  
  General Arguments:
      --PrintGlobals:              Print the list of globals.
      --PrintGroups:               Print the list of groups.
      --PrintGroup=[group]:        Print all TypeIds of group.
      --PrintTypeIds:              Print all TypeIds.
      --PrintAttributes=[typeid]:  Print all attributes of typeid.
      --PrintHelp:                 Print this help message.

..
	This default (use of IPv4, since useIpv6 is false) can be changed by 
	toggling the boolean value as follows:
	
Αυτή η προεπιλογή (η χρήση του IPv4, καθώς η useIpv6 έχει τεθεί ως false)
μπορεί να αλλάξει μέσω εναλλαγής της δυαδικής τιμής της ως ακολούθως:

::

  ./waf --run "seventh --useIpv6=1"

.. and have a look at the pcap generated, such as with ``tcpdump``:

και δείτε το PCAP αρχείο που έχει δημιουργηθεί, για παράδειγμα με την
εντολή ``tcpdump``:

::

  tcpdump -r seventh.pcap -nn -tt

..
	This has been a short digression into IPv6 support and the command line,
	which was also introduced earlier in this tutorial.  For a dedicated 
	example of command line usage, please see 
	``src/core/examples/command-line-example.cc``.
	
Αυτή ήταν μια σύντομη παρέκβαση σχετικά με την υποστήριξη του IPv6 και την
γραμμή εντολών, η οποία επίσης παρουσιάστηκε νωρίτερα σε αυτόν τον οδηγό. Για 
ένα παράδειγμα με εξ ολοκλήρου χρήση της γραμμής εντολών, σας παρακαλούμε να δείτε
το ``src/core/examples/command-line-example.cc``.

..
	Now back to data collection.  In the ``examples/tutorial/`` directory, 
	type the following command: ``diff -u sixth.cc seventh.cc``, and examine
	some of the new lines of this diff:
	
Τώρα πίσω στη συλλογή δεδομένων. Στον κατάλογο ``examples/tutorial/``, 
πληκτρολογήστε την ακόλουθη εντολή: ``diff -u sixth.cc seventh.cc``, και εξετάστε
κάποιες από τις νέες γραμμές αυτής της diff:

::

  +  std::string probeType;
  +  std::string tracePath;
  +  if (useV6 == false)
  +    {
     ...
  +      probeType = "ns3::Ipv4PacketProbe";
  +      tracePath = "/NodeList/*/$ns3::Ipv4L3Protocol/Tx";
  +    }
  +  else
  +    {
     ...
  +      probeType = "ns3::Ipv6PacketProbe";
  +      tracePath = "/NodeList/*/$ns3::Ipv6L3Protocol/Tx";
  +    }
   ...
  +   // Use GnuplotHelper to plot the packet byte count over time
  +   GnuplotHelper plotHelper;
  + 
  +   // Configure the plot.  The first argument is the file name prefix
  +   // for the output files generated.  The second, third, and fourth
  +   // arguments are, respectively, the plot title, x-axis, and y-axis labels
  +   plotHelper.ConfigurePlot ("seventh-packet-byte-count",
  +                             "Packet Byte Count vs. Time",
  +                             "Time (Seconds)",
  +                             "Packet Byte Count");
  + 
  +   // Specify the probe type, trace source path (in configuration namespace), and
  +   // probe output trace source ("OutputBytes") to plot.  The fourth argument
  +   // specifies the name of the data series label on the plot.  The last
  +   // argument formats the plot by specifying where the key should be placed.
  +   plotHelper.PlotProbe (probeType,
  +                         tracePath,
  +                         "OutputBytes",
  +                         "Packet Byte Count",
  +                         GnuplotAggregator::KEY_BELOW);
  + 
  +   // Use FileHelper to write out the packet byte count over time
  +   FileHelper fileHelper;
  + 
  +   // Configure the file to be written, and the formatting of output data.
  +   fileHelper.ConfigureFile ("seventh-packet-byte-count",
  +                             FileAggregator::FORMATTED);
  + 
  +   // Set the labels for this formatted output file.
  +   fileHelper.Set2dFormat ("Time (Seconds) = %.3e\tPacket Byte Count = %.0f");
  + 
  +   // Specify the probe type, probe path (in configuration namespace), and
  +   // probe output trace source ("OutputBytes") to write.
  +   fileHelper.WriteProbe (probeType,
  +                          tracePath,
  +                          "OutputBytes");
  + 
      Simulator::Stop (Seconds (20));
      Simulator::Run ();
      Simulator::Destroy ();
  

..
	The careful reader will have noticed, when testing the IPv6 command
	line attribute above, that ``seventh.cc`` had created a number of new output files:
	
Ο προσεκτικός αναγνώστης θα 'χει ήδη παρατηρήσει ότι, όταν δοκιμάζαμε την
ιδιότητα σχετικά με το IPv6 στη γραμμή εντολών παραπάνω, εκείνο το αρχείο
``seventh.cc`` είχε δημιουργήσει αρκετά νέα αρχεία εξόδου:

::

  seventh-packet-byte-count-0.txt
  seventh-packet-byte-count-1.txt
  seventh-packet-byte-count.dat
  seventh-packet-byte-count.plt
  seventh-packet-byte-count.png
  seventh-packet-byte-count.sh

..
	These were created by the additional statements introduced above; in
	particular, by a GnuplotHelper and a FileHelper.  This data was produced
	by hooking the data collection components to |ns3| trace sources, and
	marshaling the data into a formatted ``gnuplot`` and into a formatted
	text file.  In the next sections, we'll review each of these.
	
Αυτά δημιουργήθηκαν από τις πρόσθετες δηλώσεις που εισήχθησαν παραπάνω. Πιο συγκεκριμένα,
από έναν GnuplotHelper και έναν FileHelper. Αυτά τα δεδομένα παρήχθησαν συνδέοντας
τα μέρη για τη συλλογή δεδομένων σε πηγές ιχνών του |ns3|, και μέσω εισαγωγής των
δεδομένων σε ένα ήδη διαμορφωμένο ``gnuplot`` και σε ένα διαμορφωμένο αρχείο κειμένου.
Στα επόμενα τμήματα, θα εξετάσουμε κάθε ένα από αυτά.

.. _GnuPlotHelper:

GnuplotHelper
*************

..
	The GnuplotHelper is an |ns3| helper object aimed at the production of
	``gnuplot`` plots with as few statements as possible, for common cases.
	It hooks |ns3| trace sources with data types supported by the 
	data collection system.  Not all |ns3| trace sources data types are 
	supported, but many of the common trace types are, including TracedValues 
	with plain old data (POD) types.
	
O GnuplotHelper είναι ένα αντικείμενο-βοηθός του |ns3| που στοχεύει στην 
παραγωγή γραφικών παραστάσεων ``gnuplot`` με όσο το δυνατόν λιγότερες δηλώσεις γίνεται,
για συνηθισμένες περιπτώσεις. Συνδέει πηγές ιχνών του |ns3| με τύπους δεδομένων που 
υποστηρίζονται από το σύστημα συλλογής δεδομένων. Δεν υποστηρίζονται όλοι οι τύποι
δεδομένων που αφορούν τις πηγές ιχνών του |ns3|, αλλά υποστηρίζονται πολλοί από τους συνηθισμένους
τύπους ιχνηλασίας, συμπεριλαμβανομένων των TracedValues μαζί με
τύπους απλών παλιών δεδομένων (plain old data ή POD).

.. Let's look at the output produced by this helper:

Ας δούμε την έξοδο που προκύπτει από αυτόν τον βοηθό:

::
  
  seventh-packet-byte-count.dat
  seventh-packet-byte-count.plt
  seventh-packet-byte-count.sh

..
	The first is a gnuplot data file with a series of space-delimited 
	timestamps and packet byte counts.  We'll cover how this particular
	data output was configured below, but let's continue with the output
	files.  The file ``seventh-packet-byte-count.plt`` is a gnuplot plot file,
	that can be opened from within gnuplot.  Readers who understand gnuplot
	syntax can see that this will produce a formatted output PNG file named
	``seventh-packet-byte-count.png``.   Finally, a small shell script
	``seventh-packet-byte-count.sh`` runs this plot file through gnuplot
	to produce the desired PNG (which can be viewed in an image editor); that
	is, the command:
	
Το πρώτο είναι ένα αρχείο δεδομένων gnuplot με μια σειρά από χρονοσημάνσεις
διαχωρισμένες με κενά και καταμετρήσεις των byte των πακέτων. Θα καλύψουμε παρακάτω 
το πως καθορίστηκε η συγκεκριμένη έξοδος δεδομένων, αλλά τώρα ας συνεχίσουμε με τα
αρχεία εξόδου. Το αρχείο ``seventh-packet-byte-count.plt`` είναι ένα αρχείο γραφικής
παράστασης gnuplot, που μπορεί να ανοιχτεί μέσω του gnuplot. Οι αναγνώστες που κατανοούν
τη σύνταξη του gnuplot μπορούν να δουν ότι αυτό θα παράξει ένα διαμορφωμένο PNG αρχείο ως
έξοδο με το όνομα ``seventh-packet-byte-count.png``. Τέλος, ένα μικρό σενάριο κελύφους
``seventh-packet-byte-count.sh`` εκτελεί αυτό το αρχείο γραφικής παράστασης μέσω του gnuplot
για να παράξει το επιθυμητό PNG (το οποίο μπορείτε να δείτε μέσω κάποιου επεξεργαστή εικόνων).
Πρόκειται για την εντολή:

:: 

  sh seventh-packet-byte-count.sh

..
	will yield ``seventh-packet-byte-count.png``.  Why wasn't this PNG
	produced in the first place?  The answer is that by providing the 
	plt file, the user can hand-configure the result if desired, before
	producing the PNG.
	
που θα παράξει το ``seventh-packet-byte-count.png``. Γιατί δεν παρήχθη 
εξαρχής αυτό το αρχείο PNG; Η απάντηση σε αυτό είναι ότι παρέχοντας το αρχείο
plot, ο χρήστης μπορεί να ρυθμίσει χειροκίνητα το αποτέλεσμα εάν το επιθυμεί,
πριν να παραχθεί το PNG.

..
	The PNG image title states that this plot is a plot of 
	"Packet Byte Count vs. Time", and that it is plotting the probed data
	corresponding to the trace source path:
	
Ο τίτλος της PNG εικόνας δηλώνει ότι η γραφική παράσταση είναι μια παράσταση
"Καταμέτρησης Byte Πακέτων προς Χρόνο", και ότι σχεδιάζει τα δεδομένα που έχουν
ανιχνευθεί σε αντιστοιχία με το μονοπάτι της πηγής ιχνηλασίας: 

::

  /NodeList/*/$ns3::Ipv6L3Protocol/Tx

..
	Note the wild-card in the trace path.  In summary, what this plot is 
	capturing is the plot of packet bytes observed at the transmit trace
	source of the Ipv6L3Protocol object; largely 596-byte TCP segments
	in one direction, and 60-byte TCP acks in the other (two node
	trace sources were matched by this trace source).
	
Σημειώστε τον χαρακτήρα-μπαλαντέρ στο μονοπάτι των ιχνών. Συνολικά, αυτό
που αποτυπώνει αυτή η γραφική παράσταση είναι η αναπαράσταση των byte των πακέτων
που παρατηρούνται στην πηγή ιχνών μετάδοσης του αντικειμένου Ipv6L3Protocol: σε
μεγάλο βαθμό TCP πακέτα των 596 byte στη μια κατεύθυνση, και TCP επιβεβαιώσεις των 60 byte
στην άλλη (σε αυτή την πηγή ιχνών αντιστοιχήθηκαν πηγές ιχνών από δύο κόμβους). 

..
	How was this configured?  A few statements need to be provided.  First,
	the GnuplotHelper object must be declared and configured:
	
Πώς καθορίστηκε αυτό; Χρειάζονται μερικές δηλώσεις. Αρχικά, το αντικείμενο
GnuplotHelper πρέπει να δηλωθεί και να ρυθμιστεί:

::
  
  +  // Use GnuplotHelper to plot the packet byte count over time
  +  GnuplotHelper plotHelper;
  +
  +  // Configure the plot.  The first argument is the file name prefix
  +  // for the output files generated.  The second, third, and fourth
  +  // arguments are, respectively, the plot title, x-axis, and y-axis labels
  +  plotHelper.ConfigurePlot ("seventh-packet-byte-count",
  +                            "Packet Byte Count vs. Time",
  +                            "Time (Seconds)",
  +                            "Packet Byte Count");


..
	To this point, an empty plot has been configured.  The filename prefix
	is the first argument, the plot title is the second, the x-axis label
	the third, and the y-axis label the fourth argument.
	
Μέχρι αυτό το σημείο, έχει ρυθμιστεί μια άδεια γραφική παράσταση. Το πρόθεμα του
ονόματος του αρχείου είναι το πρώτο όρισμα, ο τίτλος της γραφικής είναι το δεύτερο,
η ετικέτα του άξονα των Χ είναι το τρίτο, και η ετικέτα του άξονα των Υ το τέταρτο
όρισμα.

..
	The next step is to configure the data, and here is where the trace
	source is hooked.  First, note above in the program we declared a few 
	variables for later use:
	
Το επόμενο βήμα είναι να καθορίσουμε τα δεδομένα, και εδώ είναι που συνδέεται
η πηγή ιχνών. Αρχικά, σημειώστε παραπάνω ότι στο πρόγραμμα δηλώσαμε μερικές
μεταβλητές για μετέπειτα χρήση:

::

  +  std::string probeType;
  +  std::string tracePath;
  +  probeType = "ns3::Ipv6PacketProbe";
  +  tracePath = "/NodeList/*/$ns3::Ipv6L3Protocol/Tx";

.. We use them here:

Τις χρησιμοποιούμε εδώ:

::
   
  +  // Specify the probe type, trace source path (in configuration namespace), and
  +  // probe output trace source ("OutputBytes") to plot.  The fourth argument
  +  // specifies the name of the data series label on the plot.  The last
  +  // argument formats the plot by specifying where the key should be placed.
  +  plotHelper.PlotProbe (probeType,
  +                        tracePath,
  +                        "OutputBytes",
  +                        "Packet Byte Count",
  +                        GnuplotAggregator::KEY_BELOW);

..
	The first two arguments are the name of the probe type and the trace source path.
	These two are probably the hardest to determine when you try to use
	this framework to plot other traces.  The probe trace here is the ``Tx``
	trace source of class ``Ipv6L3Protocol``.  When we examine this class
	implementation (``src/internet/model/ipv6-l3-protocol.cc``) we can
	observe:

Τα πρώτα δύο ορίσματα είναι το όνομα του τύπου probe και το μονοπάτι της πηγής ιχνών.
Αυτά τα δύο είναι μάλλον τα δυσκολότερα να οριστούν, όταν προσπαθείτε να χρησιμοποιήσετε
αυτό το πλαίσιο εργασίας για να σχεδιάσετε άλλα ίχνη. Το probe ίχνος εδώ είναι η ``Tx``
πηγή ιχνών της κλάσης ``Ipv6L3Protocol``. Αν εξετάσουμε την υλοποίηση αυτής της κλάσης
(``src/internet/model/ipv6-l3-protocol.cc``), θα παρατηρήσουμε το:

::

    .AddTraceSource ("Tx", "Send IPv6 packet to outgoing interface.",
                     MakeTraceSourceAccessor (&Ipv6L3Protocol::m_txTrace))

..
	This says that ``Tx`` is a name for variable ``m_txTrace``, which has
	a declaration of:
	
Αυτό μας λέει ότι το ``Tx`` είναι ένα όνομα για τη μεταβλητή ``m_txTrace``,
η οποία έχει μια δήλωση ως εξής:

::

  /**
   * \brief Callback to trace TX (transmission) packets.
   */
  TracedCallback<Ptr<const Packet>, Ptr<Ipv6>, uint32_t> m_txTrace;

..
	It turns out that this specific trace source signature is supported
	by a Probe class (what we need here) of class Ipv6PacketProbe.
	See the files ``src/internet/model/ipv6-packet-probe.{h,cc}``.
	
Προκύπτει ότι αυτή η συγκεκριμένη υπογραφή πηγής ιχνών υποστηρίζεται από μια 
κλάση Probe (αυτό που χρειαζόμαστε εδώ) της κλάσης Ipv6PacketProbe. Δείτε τα
αρχεία ``src/internet/model/ipv6-packet-probe.{h,cc}``.

..
	So, in the PlotProbe statement above, we see that the statement is
	hooking the trace source (identified by path string) with a matching
	|ns3| Probe type of ``Ipv6PacketProbe``.  If we did not support
	this probe type (matching trace source signature), we could have not
	used this statement (although some more complicated lower-level statements
	could have been used, as described in the manual).

Έτσι, στην παραπάνω δήλωση PlotProbe, βλέπουμε ότι η δήλωση συνδέει την πηγή
ιχνών (που ταυτοποιείται από την συμβολοσειρά του μονοπατιού) με έναν αντίστοιχο
τύπο |ns3| Probe του ``Ipv6PacketProbe``. Εάν δεν υποστηρίζαμε αυτόν τον τύπο probe
(ώστε να αντιστοιχίζεται με την υπογραφή της πηγής ιχνών), δε θα μπορούσαμε 
να χρησιμοποιήσουμε αυτή τη δήλωση (παρόλο που θα μπορούσαν να χρησιμοποιηθούν μερικές
πιο πολύπλοκες και χαμηλότερου επιπέδου δηλώσεις, όπως περιγράφεται στο εγχειρίδιο).

..
	The Ipv6PacketProbe exports, itself, some trace sources that extract
	the data out of the probed Packet object:
	
Η Ipv6PacketProbe εξάγει, η ίδια, μερικές πηγές ιχνών που συλλέγουν τα δεδομένα
από το ανιχνευθέν αντικείμενο Packet:

::
  
  TypeId
  Ipv6PacketProbe::GetTypeId ()
  {
    static TypeId tid = TypeId ("ns3::Ipv6PacketProbe")
      .SetParent<Probe> ()
      .AddConstructor<Ipv6PacketProbe> ()
      .AddTraceSource ( "Output",
                        "The packet plus its IPv6 object and interface that serve as the output for this probe",
                        MakeTraceSourceAccessor (&Ipv6PacketProbe::m_output))
      .AddTraceSource ( "OutputBytes",
                        "The number of bytes in the packet",
                        MakeTraceSourceAccessor (&Ipv6PacketProbe::m_outputBytes))
    ;
    return tid;
  }
  

..
	The third argument of our PlotProbe statement specifies that we are
	interested in the number of bytes in this packet; specifically, the
	"OutputBytes" trace source of Ipv6PacketProbe.
	Finally, the last two arguments of the statement provide the plot
	legend for this data series ("Packet Byte Count"), and an optional
	gnuplot formatting statement (GnuplotAggregator::KEY_BELOW) that we want 
	the plot key to be inserted below the plot.  Other options include
	NO_KEY, KEY_INSIDE, and KEY_ABOVE.
	
Το τρίτο όρισμα της PlotProbe δήλωσής μας προσδιορίζει ότι ενδιαφερόμαστε
για έναν αριθμό από byte σε αυτό το πακέτο: ειδικότερα, στην πηγή ιχνών 
"OutputBytes" της Ipv6PacketProbe.
Τέλος, τα δύο τελευταία ορίσματα της δήλωσης παρέχουν το υπόμνημα της γραφικής παράστασης
για αυτή τη σειρά δεδομένων ("Packet Byte Count"), και μία προαιρετική δήλωση
μορφοποίησης του gnuplot (GnuplotAggregator::KEY_BELOW) ότι θέλουμε το υπόμνημα
της γραφικής παράστασης να εισαχθεί κάτω από την παράσταση. Άλλες επιλογές
περιλαμβάνουν τα NO_KEY, KEY_INSIDE, και KEY_ABOVE.


.. Supported Trace Types

Υποστηριζόμενοι Τύποι Ιχνών
***************************

.. The following traced values are supported with Probes as of this writing:

Οι ακόλουθες καταγεγραμμένες τιμές υποστηρίζονται από Probes έως και τη στιγμή που γράφεται
το παρόν κείμενο: 

  +-------------------+-------------------+------------------------------------+
  | Τύπος TracedValue |   Τύπος Probe     |  Αρχείο                            |
  +===================+=========+=========+====================================+
  | double            | DoubleProbe       | stats/model/double-probe.h         |
  +-------------------+-------------------+------------------------------------+
  | uint8_t           | Uinteger8Probe    | stats/model/uinteger-8-probe.h     |
  +-------------------+-------------------+------------------------------------+
  | uint16_t          | Uinteger16Probe   | stats/model/uinteger-16-probe.h    |
  +-------------------+-------------------+------------------------------------+
  | uint32_t          | Uinteger32Probe   | stats/model/uinteger-32-probe.h    |
  +-------------------+-------------------+------------------------------------+
  | bool              | BooleanProbe      | stats/model/uinteger-16-probe.h    |
  +-------------------+-------------------+------------------------------------+
  | ns3::Time         | TimeProbe         | stats/model/time-probe.h           |
  +-------------------+-------------------+------------------------------------+

.. The following TraceSource types are supported by Probes as of this writing:

Οι ακόλουθοι τύποι TraceSource υποστηρίζονται από τα Probes έως και τη στιγμή που γράφεται
το παρόν κείμενο: 

  +------------------------------------------+------------------------+---------------+----------------------------------------------------+
  | Tύπος TracedSource                       |   Τύπος Probe          | Έξοδος Probe  |  Αρχείο                                            |
  +==========================================+========================+===============+====================================================+
  | Ptr<const Packet>                        | PacketProbe            | OutputBytes   | network/utils/packet-probe.h                       |
  +------------------------------------------+------------------------+---------------+----------------------------------------------------+
  | Ptr<const Packet>, Ptr<Ipv4>, uint32_t   | Ipv4PacketProbe        | OutputBytes   | internet/model/ipv4-packet-probe.h                 |
  +------------------------------------------+------------------------+---------------+----------------------------------------------------+
  | Ptr<const Packet>, Ptr<Ipv6>, uint32_t   | Ipv6PacketProbe        | OutputBytes   | internet/model/ipv6-packet-probe.h                 |
  +------------------------------------------+------------------------+---------------+----------------------------------------------------+
  | Ptr<const Packet>, Ptr<Ipv6>, uint32_t   | Ipv6PacketProbe        | OutputBytes   | internet/model/ipv6-packet-probe.h                 |
  +------------------------------------------+------------------------+---------------+----------------------------------------------------+
  | Ptr<const Packet>, const Address&        | ApplicationPacketProbe | OutputBytes   | applications/model/application-packet-probe.h      |
  +------------------------------------------+------------------------+---------------+----------------------------------------------------+

..
	As can be seen, only a few trace sources are supported, and they are all 
	oriented towards outputting the Packet size (in bytes).  However,
	most of the fundamental data types available as TracedValues can be
	supported with these helpers.
	
Όπως είναι φανερό, μόνο μερικές πηγές ιχνών υποστηρίζονται, και όλες προσανατολίζονται
στην εξαγωγή του μεγέθους του Packet (σε byte). Ωστόσο, οι περισσότεροι
από τους βασικούς τύπους δεδομένων που είναι διαθέσιμοι ως TracedValues μπορούν
να υποστηριχθούν από αυτούς τους βοηθούς.

FileHelper
**********

..
	The FileHelper class is just a variation of the previous GnuplotHelper
	example.  The example program provides formatted output of the
	same timestamped data, such as follows:
	
Η κλάση FileHelper είναι απλά μια παραλλαγή του προηγούμενου παραδείγματος με
τον GnuplotHelper. Το εν λόγω πρόγραμμα παρέχει διαμορφωμένη έξοδο των
ίδιων χρονοσημασμένων δεδομένων, όπως η παρακάτω:

::
  
  Time (Seconds) = 9.312e+00	Packet Byte Count = 596
  Time (Seconds) = 9.312e+00	Packet Byte Count = 564

..
	Two files are provided, one for node "0" and one for node "1" as can
	be seen in the filenames.  Let's look at the code piece-by-piece:
	
Δύο αρχεία παρέχονται, ένα για τον κόμβο "0" και ένα για τον κόμβο "1", όπως
μπορείτε να δείτε στα ονόματα των αρχείων. Ας δούμε τον κώδικα κομμάτι-κομμάτι:

::

  +   // Use FileHelper to write out the packet byte count over time
  +   FileHelper fileHelper;
  + 
  +   // Configure the file to be written, and the formatting of output data.
  +   fileHelper.ConfigureFile ("seventh-packet-byte-count",
  +                             FileAggregator::FORMATTED);

..
	The file helper file prefix is the first argument, and a format specifier
	is next.
	Some other options for formatting include SPACE_SEPARATED, COMMA_SEPARATED,
	and TAB_SEPARATED.  Users are able to change the formatting (if
	FORMATTED is specified) with a format string such as follows:  
	
Το πρόθεμα του αρχείου για τον βοηθό αρχείων είναι το πρώτο όρισμα, και ένας 
προσδιοριστής της διαμόρφωσης είναι το επόμενο. Κάποιες άλλες επιλογές διαμόρφωσης 
περιλαμβάνουν τα PACE_SEPARATED, COMMA_SEPARATED και TAB_SEPARATED. Οι χρήστες
μπορούν να αλλάξουν τη μορφοποίηση (εάν το FORMATTED καθορίζεται) με μια συμβολοσειρά
διαμόρφωσης όπως η παρακάτω:

::

  + 
  +   // Set the labels for this formatted output file.
  +   fileHelper.Set2dFormat ("Time (Seconds) = %.3e\tPacket Byte Count = %.0f");

..
	Finally, the trace source of interest must be hooked.  Again, the probeType and
	tracePath variables in this example are used, and the probe's output
	trace source "OutputBytes" is hooked:
	
Εν τέλει, πρέπει να προσδεθεί η πηγή ιχνών που μας ενδιαφέρει. Πάλι, χρησιμοποιούνται
οι μεταβλητές probeType και tracePath σε αυτό το παράδειγμα, και η έξοδος της πηγής
ιχνών "OutputBytes" του probe συνδέεται:

::

  + 
  +   // Specify the probe type, trace source path (in configuration namespace), and
  +   // probe output trace source ("OutputBytes") to write.
  +   fileHelper.WriteProbe (probeType,
  +                          tracePath,
  +                          "OutputBytes");
  + 

..
	The wildcard fields in this trace source specifier match two trace sources.
	Unlike the GnuplotHelper example, in which two data series were overlaid
	on the same plot, here, two separate files are written to disk.
	
Τα πεδία χαρακτήρων-μπαλαντέρ σε αυτόν τον προσδιοριστή πηγής ιχνών αντιστοιχούν
σε δύο πηγές ιχνών. Εν αντιθέσει με το παράδειγμα του GnuplotHelper, στο οποίο
δύο σειρές δεδομένων παρατέθηκαν στην ίδια γραφική παράσταση, εδώ δύο ξεχωριστά
αρχεία αποθηκεύονται στον δίσκο. 

.. Summary

Σύνοψη
******

..
	Data collection support is new as of ns-3.18, and basic support for
	providing time series output has been added.  The basic pattern described
	above may be replicated within the scope of support of the existing
	probes and trace sources.  More capabilities including statistics
	processing will be added in future releases. 
	
Η υποστήριξη για τη συλλογή δεδομένων είναι καινούργια από την έκδοση ns-3.18, και 
έχει προστεθεί η βασική υποστήριξη για την παροχή αποτελεσμάτων σε χρονικές σειρές (time series output).
Το βασικό πρότυπο που περιγράφεται παραπάνω μπορεί να αναπαραχθεί και μέσα στα 
περιθώρια της υποστήριξης των υπάρχοντων probe και πηγών ιχνών. Περισσότερες δυνατότητες,
συμπεριλαμβανομένης της στατιστικής επεξεργασίας, θα προστεθούν σε μελλοντικές εκδόσεις.
