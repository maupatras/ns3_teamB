.. include:: replace.txt
.. highlight:: cpp
.. role:: raw-role(raw)
   :format: html latex

.. Mimic doxygen formatting for parameter names
   
.. raw:: html

    <style>.param {font-weight:bold; color:#602020;}</style>

.. role:: param


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
	> Current file was initially translated by [Vasileios Dimitropoulos].
	> Last update was performed at 28-04-2015 by [Vasileios Dimitropoulos].
	========================================================================================

..
	Tracing

Ιχνηλασία
---------

..
	Background

Ιστορικό
********

..
	As mentioned in :ref:`UsingTracingSystem`, the whole point
	of running an |ns3| simulation is to generate output for study.  You
	have two basic strategies to obtain output from |ns3|: using generic
	pre-defined bulk output mechanisms and parsing their content to
	extract interesting information; or somehow developing an output
	mechanism that conveys exactly (and perhaps only) the information
	wanted.

Όπως αναφέρεται στο :ref:`UsingTracingSystem`, το νόημα της λειτουργίας μιας προσομοίωσης |ns3| είναι να 
παράγει έξοδο για μελέτη. Έχετε δύο βασικές στρατηγικές για την απόκτηση εξόδου από |ns3|: τη χρήση γενικών 
μαζικών μηχανισμών παραγωγής και την ανάλυση του περιεχομένου τους για να εξαχθούν ενδιαφέρουσες 
πληροφορίες, ή με κάποιο τρόπο την ανάπτυξη ενός μηχανισμού εξόδου που αποπνέει ακριβώς (και ίσως μόνο) τις 
ζητούμενες πληροφορίες.

..
	Using pre-defined bulk output mechanisms has the advantage of not
	requiring any changes to |ns3|, but it may require writing scripts to
	parse and filter for data of interest.  Often, PCAP or ``NS_LOG``
	output messages are gathered during simulation runs and separately run
	through scripts that use ``grep``, ``sed`` or ``awk`` to parse the
	messages and reduce and transform the data to a manageable form.
	Programs must be written to do the transformation, so this does not
	come for free.  ``NS_LOG`` output is not considered part of the |ns3|
	API, and can change without warning between releases.  In addition,
	``NS_LOG`` output is only available in debug builds, so relying on it
	imposes a performance penalty.  Of course, if the information of
	interest does not exist in any of the pre-defined output mechanisms,
	this approach fails.

Η χρήση μαζικών μηχανισμών εξόδου έχει το πλεονέκτημα ότι δεν απαιτεί αλλαγές στον |ns3|, 
αλλά μπορεί να απαιτήσει τη συγγραφή σεναρίων για την ανάλυση και το φιλτράρισμα των δεδομένων του ενδιαφέροντος. 
Συχνά, PCAP ή ``NS_LOG`` μηνύματα εξόδου που συγκεντρώνονται κατά τη διάρκεια προσομοιώσεων τρέχουν και ξεχωριστά τρέχουν 
μέσα από σενάρια που χρησιμοποιούν ``grep``, ``sed`` ή ``awk`` για να αναλύσουν τα μηνύματα και να μειώσουν και να 
μετατρέψουν τα δεδομένα σε μία διαχειρίσιμη μορφή. Τα προγράμματα πρέπει να είναι γραμμένα ώστε να μετατρέπονται, έτσι 
αυτό δεν έρχεται δωρεάν. Η έξοδος του ``NS_LOG`` δεν θεωρείται μέρος του |ns3| API, και μπορεί να αλλάξει χωρίς 
προειδοποίηση μεταξύ των εκδόσεων. Επιπλέον, η έξοδος του ``NS_LOG`` είναι διαθέσιμη μόνο σε εκδόσεις εντοπισμού σφαλμάτων, 
έτσι επικαλούμενη επιβάλλει ποινή απόδοσης. Φυσικά, αν η πληροφορία που ενδιαφέρει δεν υπάρχει σε κανένα από 
τους προκαθορισμένους μηχανισμούς εξόδου, η προσέγγιση αυτή αποτυγχάνει.

..
	If you need to add some tidbit of information to the pre-defined bulk
	mechanisms, this can certainly be done; and if you use one of the
	|ns3| mechanisms, you may get your code added as a contribution.

Εάν χρειάζεστε να προσθέσετε μερικές εξειδικευμένες πληροφορίες στους μαζικούς μηχανισμούς, αυτό 
σίγουρα μπορεί να γίνει΄ και αν χρησιμοποιήσετε έναν από τους |ns3| μηχανισμούς, μπορείτε να πάρετε τον κώδικά σας 
προστιθέμενο ως εισφορά.
	
..
	|ns3| provides another mechanism, called Tracing, that avoids some of
	the problems inherent in the bulk output mechanisms.  It has several
	important advantages.  First, you can reduce the amount of data you
	have to manage by only tracing the events of interest to you (for
	large simulations, dumping everything to disk for post-processing can
	create I/O bottlenecks).  Second, if you use this method, you can
	control the format of the output directly so you avoid the
	postprocessing step with ``sed``, ``awk``, ``perl`` or ``python``
	scripts.  If you desire, your output can be formatted directly into a
	form acceptable by gnuplot, for example (see also
	:ref:`GnuplotHelper`).  You can add hooks in the core which can then
	be accessed by other users, but which will produce no information
	unless explicitly asked to do so.  For these reasons, we believe that
	the |ns3| tracing system is the best way to get information out of a
	simulation and is also therefore one of the most important mechanisms
	to understand in |ns3|.
	
Ο |ns3| παρέχει έναν άλλο μηχανισμό, που ονομάζεται Tracing, ο οποίος αποφεύγει ορισμένα από τα προβλήματα που συνδέονται 
με τους μαζικούς μηχανισμούς εξόδου. Έχει αρκετά σημαντικά πλεονεκτήματα. Κατ 'αρχάς, μπορείτε να μειώσετε την ποσότητα 
των δεδομένων που θα πρέπει να διαχειριστείτε από μόνο τον εντοπισμό των εκδηλώσεων που σας ενδιαφέρουν (για τις μεγάλες 
προσομοιώσεις, τοποθετώντας τα πάντα στο δίσκο για την μετα-επεξεργασία μπορεί να δημιουργήσει I / O σημεία συμφόρησης). 
Δεύτερον, αν χρησιμοποιείτε αυτή τη μέθοδο, μπορείτε να ελέγξετε τη μορφοποίηση της εξόδου άμεσα, έτσι ώστε να 
αποφευχθεί το στάδιο της μετα-επεξεργασίας με ``sed``, ``awk``, ``perl`` ή ``python`` σενάρια. Αν επιθυμείτε, η 
παραγωγή σας μπορεί να διαμορφωθεί άμεσα σε μορφή αποδεκτή από gnuplot, για παράδειγμα (βλέπε επίσης :ref:`GnuplotHelper`). 
Μπορείτε να προσθέσετε άγκιστρα στον πυρήνα που μπορούν να προσπελαστούν από άλλους χρήστες, αλλά οι
οποίες δε θα παράγουν καμία πληροφορία εκτός αν σας ζητηθεί ρητά να το πράξετε. Για αυτούς τους λόγους, πιστεύουμε ότι 
το σύστημα ανίχνευσης |ns3| είναι ο καλύτερος τρόπος για να πάρετε πληροφορίες από μια προσομοίωση και είναι επίσης, 
ως εκ τούτου ένας από τους πιο σημαντικούς μηχανισμούς για να καταλάβουμε τον |ns3|.

..
	Blunt Instruments

Εξειδικευμένα εργαλεία
+++++++++++++++++

..
	There are many ways to get information out of a program.  The most
	straightforward way is to just print the information directly to the
	standard output, as in

Υπάρχουν πολλοί τρόποι για να πάρετε πληροφορίες από ένα πρόγραμμα. Ο πιο απλός τρόπος είναι να εκτυπώσετε 
μόνο τις πληροφορίες απευθείας στην κανονική έξοδο, όπως και στην
::

  #include <iostream>
  ...
  void
  SomeFunction (void)
  {
    uint32_t x = SOME_INTERESTING_VALUE;
    ...
    std::cout << "The value of x is " << x << std::endl;
    ...
  } 

..
	Nobody is going to prevent you from going deep into the core of |ns3|
	and adding print statements.  This is insanely easy to do and, after
	all, you have complete control of your own |ns3| branch.  This will
	probably not turn out to be very satisfactory in the long term,
	though.

Κανείς δεν πρόκειται να σας αποτρέψει από το να πηγαίνετε βαθιά στον πυρήνα του |ns3| 
και προσθέτοντας τις δηλώσεις εκτύπωσης. Αυτό είναι τρομερά εύκολο να γίνει και μετά, έχετε τον πλήρη έλεγχο του δικού σας υποκατάστημα |ns3|. Αυτό πιθανόν να μην αποδειχθεί ότι είναι ικανοποιητικό σε μακροπρόθεσμη βάση,
όμως.

..
	As the number of print statements increases in your programs, the task
	of dealing with the large number of outputs will become more and more
	complicated.  Eventually, you may feel the need to control what
	information is being printed in some way, perhaps by turning on and
	off certain categories of prints, or increasing or decreasing the
	amount of information you want.  If you continue down this path you
	may discover that you have re-implemented the ``NS_LOG`` mechanism
	(see :ref:`UsingLogging`).  In order to avoid that, one of the first
	things you might consider is using ``NS_LOG`` itself.

Καθώς ο αριθμός των καταστάσεων εκτύπωσης αυξάνει στα προγράμματά σας, το έργο
της αντιμετώπισης του μεγάλου αριθμού των αποτελεσμάτων θα γίνονται όλο και περισσότερο
περίπλοκα. Τελικά, μπορεί να αισθανθείτε την ανάγκη να ελέγχετε ό,τι
πληροφορίες εκτυπώνετε με κάποιο τρόπο, ίσως ενεργοποιώντας και
απενεργοποιώντας ορισμένες κατηγορίες εκτυπώσεις, ή αυξάνοντας ή μειώνοντας το
την ποσότητα των πληροφοριών που θέλετε. Εάν συνεχίσουμε αυτήν την πορεία σας
μπορεί να ανακαλύψετε ότι έχετε εκ νέου σε εφαρμογή το μηχανισμό ``NS_LOG``
(βλέπε :ref:`UsingLogging`). Προκειμένου να αποφευχθεί αυτό, ένα από τα πρώτα
πράγματα που θα μπορούσατε να εξετάσετε είναι να χρησιμοποιείσετε μόνο του το ``NS_LOG``.

..
	We mentioned above that one way to get information out of |ns3| is to
	parse existing ``NS_LOG`` output for interesting information.  If you
	discover that some tidbit of information you need is not present in
	existing log output, you could edit the core of |ns3| and simply add
	your interesting information to the output stream.  Now, this is
	certainly better than adding your own print statements since it
	follows |ns3| coding conventions and could potentially be useful to
	other people as a patch to the existing core.

Μας αναφέρθηκε παραπάνω ότι ένας τρόπος για να πάρετε πληροφορίες από τον |ns3| είναι να αναλύσει τις υπάρχουσες 
εξόδους ``NS_LOG`` για ενδιαφέρουσες πληροφορίες. Αν ανακαλύψετε ότι χρειάζεστε κάποια εξειδικευμένη πληροφορία που 
 δεν είναι παρών σε υφιστάμενες έξοδους αρχείων καταγραφής, μπορείτε να επεξεργαστείτε τον πυρήνα 
του |ns3| και απλά προσθέστε ενδιαφέρουσες πληροφορίες σας στη ροή εξόδου. Τώρα, αυτό είναι σίγουρα καλύτερο 
από την προσθήκη των δικών σας δηλώσεων εκτύπωσης, δεδομένου ότι ακολουθεί ο |ns3| συμβάσεις κωδικοποίησης και 
θα μπορούσε ενδεχομένως να είναι χρήσιμο σε άλλους ανθρώπους ως ένα patch στον υφιστάμενο πυρήνα.

..
	Let's pick a random example.  If you wanted to add more logging to the
	|ns3| TCP socket (``tcp-socket-base.cc``) you could just add a new
	message down in the implementation.  Notice that in
	``TcpSocketBase::ReceivedAck()`` there is no log message for the no ACK
	case.  You could simply add one, changing the code.  Here is the original
	
Ας πάρουμε ένα τυχαίο παράδειγμα. Αν θέλετε να προσθέσετε περισσότερη καταγραφή στον |ns3| υποδοχή TCP 
(``tcp-socket-base.cc``) θα μπορούσατε απλά να προσθέσετε ένα νέο μήνυμα κάτω στην εφαρμογή. Σημειώστε 
ότι στο ``TcpSocketBase::ReceivedAck()`` δεν υπάρχει log μήνυμα για την περίπτωση του no ACK. Θα μπορούσατε 
απλά να προσθέσετε ένα, αλλάζοντας τον κώδικα. Εδώ είναι η αρχική 
::

  /** Process the newly received ACK */
  void
  TcpSocketBase::ReceivedAck (Ptr<Packet> packet, const TcpHeader& tcpHeader)
  {
    NS_LOG_FUNCTION (this << tcpHeader);

    // Received ACK. Compare the ACK number against highest unacked seqno
    if (0 == (tcpHeader.GetFlags () & TcpHeader::ACK))
      { // Ignore if no ACK flag
      }
    ...

..
	To log the no ACK case, you can add a new ``NS_LOG_LOGIC`` in the
	``if`` statement body
	
Για να συνδεθείτε στην περίπτωση του no ACK, μπορείτε να προσθέσετε ένα νέο `` NS_LOG_LOGIC`` στο `` if`` σώμα δήλωσης
::

  /** Process the newly received ACK */
  void
  TcpSocketBase::ReceivedAck (Ptr<Packet> packet, const TcpHeader& tcpHeader)
  {
    NS_LOG_FUNCTION (this << tcpHeader);

    // Received ACK. Compare the ACK number against highest unacked seqno
    if (0 == (tcpHeader.GetFlags () & TcpHeader::ACK))
      { // Ignore if no ACK flag
        NS_LOG_LOGIC ("TcpSocketBase " << this << " no ACK flag");
      }
    ...

..
	This may seem fairly simple and satisfying at first glance, but
	something to consider is that you will be writing code to add
	``NS_LOG`` statements and you will also have to write code (as in
	``grep``, ``sed`` or ``awk`` scripts) to parse the log output in order
	to isolate your information.  This is because even though you have
	some control over what is output by the logging system, you only have
	control down to the log component level, which is typically an entire
	source code file.

Αυτό μπορεί να φαίνεται αρκετά απλό και ικανοποιητικό με την πρώτη ματιά, αλλά κάτι που πρέπει να δούμε είναι ότι θα 
πρέπει να γράψετε κώδικα για να προσθέσετε δηλώσεις ``NS_LOG`` και θα πρέπει επίσης να γράψετε κώδικα (όπως 
στο ``grep``, ``sed`` ή ``awk`` σενάρια) για να αναλύσει το αρχείο καταγραφής εξόδου, προκειμένου να 
απομονώσουν τα στοιχεία σας. Αυτό οφείλεται στο γεγονός ότι, ακόμη και αν έχετε κάποιο έλεγχο πάνω στο τι 
είναι η έξοδος από το σύστημα καταγραφής, έχετε μόνο τον έλεγχο στο συγκεκριμένο επίπεδο log, το οποίο 
είναι συνήθως ένα ολόκληρο αρχείο πηγαίου κώδικα.

..
	If you are adding code to an existing module, you will also have to
	live with the output that every other developer has found interesting.
	You may find that in order to get the small amount of information you
	need, you may have to wade through huge amounts of extraneous messages
	that are of no interest to you.  You may be forced to save huge log
	files to disk and process them down to a few lines whenever you want
	to do anything.

Αν θέλετε να προσθέσετε κώδικα σε μια υπάρχουσα μονάδα, θα πρέπει επίσης να ακολουθείτε την έξοδο που κάθε άλλος 
προγραμματιστής έχει βρει ενδιαφέρουσα. Μπορείτε να διαπιστώσετε ότι, προκειμένου να πάρετε τις λίγες πληροφορίες 
που χρειάζεστε, μπορεί να χρειαστείτε να εντρυφήσετε μέσα από την τεράστια ποσότητα μηνυμάτων που προέρχονται από ξένα 
μηνύματα που δεν παρουσιάζουν κανένα ενδιαφέρον για εσάς. Μπορεί να αναγκαστείτε να αποθηκεύσετε τεράστια αρχεία 
καταγραφής στο δίσκο και να τα επεξεργαστείτε με σκοπό να κάνετε την δουλειά σας.

..
	Since there are no guarantees in |ns3| about the stability of
	``NS_LOG`` output, you may also discover that pieces of log output 
	which you depend on disappear or change between releases.  If you depend
	on the structure of the output, you may find other messages being
	added or deleted which may affect your parsing code.

Δεδομένου ότι δεν υπάρχουν εγγυήσεις στον |ns3| σχετικά με τη σταθερότητα της εξόδου ``NS_LOG``, μπορείτε 
επίσης να ανακαλύψετε ότι τα κομμάτια της παραγωγής εξόδου τα οποία είναι για εξαφάνιση ή για αλλαγή
μεταξύ διαφορετικών εκδόσεων. Αν εξαρτάστε στη δομή της παραγωγής, μπορείτε να βρείτε 
και άλλα μηνύματα που προστίθενται ή διαγράφονται τα οποία μπορεί να επηρεάσουν την ανάλυση του κώδικα.

..
	Finally, ``NS_LOG`` output is only available in debug builds, you
	can't get log output from optimized builds, which run about twice as
	fast.  Relying on ``NS_LOG`` imposes a performance penalty.

Τέλος, η έξοδος ``NS_LOG`` είναι διαθέσιμη μόνο σε εκδόσεις εντοπισμού σφαλμάτων, δεν μπορείτε να πάρετε συνδεθείτε 
εξόδου από βελτιστοποιημένη χτίζει, που τρέχουν περίπου δύο φορές πιο γρήγορα. Στηριζόμενη στην ``NS_LOG`` επιβάλλει 
ποινή απόδοσης.

..
	For these reasons, we consider prints to ``std::cout`` and ``NS_LOG``
	messages to be quick and dirty ways to get more information out of
	|ns3|, but not suitable for serious work.
	
Για τους λόγους αυτούς, θεωρούμε τις εκτυπώσεις στο ``std::cout`` και τα μηνύματα ``NS_LOG`` να είναι γρήγορα και απλοί 
τρόποι για να πάρετε περισσότερες πληροφορίες από τον |ns3|, αλλά δεν είναι κατάλληλο για σοβαρή δουλειά.

..
	It is desirable to have a stable facility using stable APIs that allow
	one to reach into the core system and only get the information
	required.  It is desirable to be able to do this without having to
	change and recompile the core system.  Even better would be a system
	that notified user code when an item of interest changed or an
	interesting event happened so the user doesn't have to actively poke
	around in the system looking for things.

Είναι επιθυμητό να έχουμε μια σταθερή εγκατάσταση, χρησιμοποιώντας σταθερά APIs που επιτρέπουν σε κάποιον να 
φτάσει στον πυρήνα του συστήματος και να πάρει μόνο τις πληροφορίες που απαιτούνται. Είναι επιθυμητό να είναι 
σε θέση να το κάνει αυτό χωρίς να χρειάζεται να αλλάξει και να μεταγλωττίσει ξανά τον πυρήνα του συστήματος. 
Ακόμα καλύτερα θα είναι ένα σύστημα που κοινοποίησε τον κωδικό χρήστη, όταν ένα στοιχείο του ενδιαφέροντος αλλάξει 
ή μια ενδιαφέρουσα εκδήλωση έγινε έτσι ο χρήστης θα έχει αυτά που του χρειάζονται.

..
	The |ns3| tracing system is designed to work along those lines and is
	well-integrated with the :ref:`Attribute <Attribute>` and :ref:`Config
	<Config>` subsystems allowing for relatively simple use scenarios.

Το σύστημα εντοπισμού του |ns3| έχει σχεδιαστεί για να λειτουργεί προς αυτή την κατεύθυνση και είναι καλά ενσωματωμένο 
με το :ref:`Attribute <Attribute>` και :ref:`Config <Config>` υποσυστήματα που επιτρέπουν την σχετικά 
απλή χρήση σεναρίων.

..
	Overview

Επισκόπηση
**********

..
	The |ns3| tracing system is built on the concepts of independent
	tracing sources and tracing sinks, along with a uniform mechanism for
	connecting sources to sinks.

Το σύστημα ανίχνευσης του |ns3| είναι χτισμένο στις έννοιες των ανεξάρτητων απο τον εντοπισμό πηγών και 
τον εντοπισμό συλλεκτών, μαζί με ένα ενιαίο μηχανισμό για τη σύνδεση πηγών σε συλλέκτες.

..
	Trace sources are entities that can signal events that happen in a
	simulation and provide access to interesting underlying data.  For
	example, a trace source could indicate when a packet is received by a
	net device and provide access to the packet contents for interested
	trace sinks.  A trace source might also indicate when an interesting
	state change happens in a model.  For example, the congestion window
	of a TCP model is a prime candidate for a trace source.  Every time
	the congestion window changes connected trace sinks are notified with
	the old and new value.

Πηγές εντοπισμού είναι οντότητες που μπορούν να σηματοδοτήσουν τα γεγονότα που συμβαίνουν σε μια προσομοίωση και να 
παρέχουν πρόσβαση σε ενδιαφέροντα υποκείμενα δεδομένα. Για παράδειγμα, μια πηγή ίχνους θα μπορούσε να υποδείξει 
πότε ένα πακέτο παραλαμβάνεται από μια συσκευή δικτύου και να παρέχει πρόσβαση στα περιεχόμενα του πακέτου για τους 
ενδιαφερόμενους συλλέκτες εντοπισμού. Μια πηγή ίχνους μπορεί επίσης να αναφέρει πότε μια ενδιαφέρουσα αλλαγή κατάστασης 
συμβαίνει σε ένα μοντέλο. Για παράδειγμα, το παράθυρο συμφόρησης του μοντέλου TCP είναι πρώτος υποψήφιος για μια 
πηγή ίχνους. Κάθε φορά που αλλάζει το παράθυρο συμφόρησης που είναι συνδεδεμένο με συλλέκτες ίχνους ενημερώνεται με την παλαιά 
και νέα τιμή.

..
	Trace sources are not useful by themselves; they must be connected to
	other pieces of code that actually do something useful with the
	information provided by the source.  The entities that consume trace
	information are called trace sinks.  Trace sources are generators of
	data and trace sinks are consumers.  This explicit division allows
	for large numbers of trace sources to be scattered around the system
	in places which model authors believe might be useful.  Inserting
	trace sources introduces a very small execution overhead.

Οι πηγές εντοπισμού δεν είναι χρήσιμες από μόνες τους. Θα πρέπει να συνδεθούν με άλλα κομμάτια του κώδικα που κάνουν πραγματικά 
κάτι χρήσιμο με τις πληροφορίες που παρέχονται από την πηγή. Οι οντότητες που καταναλώνουν πληροφορίες ίχνους 
ονομάζονται συλλέκτες ίχνους. Οι πηγές εντοπισμού είναι γεννήτριες των δεδομένων και οι συλλέκτες ίχνους είναι οι καταναλωτές. 
Αυτή η ρητή κατανομή επιτρέπει για ένα μεγάλο αριθμό πηγών ίχνους να είναι διάσπαρτα σε όλο το σύστημα σε χώρους 
που συγγραφείς μοντέλων πιστεύουν ότι μπορεί να είναι χρήσιμο. Η τοποθέτηση πηγών ίχνους εισάγει μία πολύ μικρή γενικά 
εκτέλεση.

..
	There can be zero or more consumers of trace events generated by a
	trace source.  One can think of a trace source as a kind of
	point-to-multipoint information link.  Your code looking for trace
	events from a particular piece of core code could happily coexist with
	other code doing something entirely different from the same
	information.

Μπορεί να υπάρχουν μηδέν ή περισσότεροι καταναλωτές από ίχνη γεγονότων που παράγεται από μια πηγή ίχνους. Κάποιος μπορεί να 
σκεφτεί μια πηγή ίχνους, ως ένα είδος point-to-multipoint σύνδεσης πληροφοριών. Ο κώδικάς σας ψάχνει για ίχνη 
γεγονότων από ένα συγκεκριμένο κομμάτι του πηγαίου κώδικα, θα μπορούσε ευτυχώς να συνυπάρχει με άλλους κώδικες να κάνει κάτι 
εντελώς διαφορετικό από την ίδια πληροφορία.

..
	Unless a user connects a trace sink to one of these sources, nothing
	is output.  By using the tracing system, both you and other people
	hooked to the same trace source are getting exactly what they want and
	only what they want out of the system.  Neither of you are impacting
	any other user by changing what information is output by the system.
	If you happen to add a trace source, your work as a good open-source
	citizen may allow other users to provide new utilities that are
	perhaps very useful overall, without making any changes to the |ns3|
	core.

Αν ένας χρήστης δεν συνδέσει ένα συλλέκτη ίχνους σε μια από αυτές τις πηγές, τίποτα δεν θα υπάρχει στην έξοδο. 
Με τη χρήση του συστήματος εντοπισμού, τόσο εσείς όσο και άλλοι άνθρωποι είναι συνδεδεμένοι με την ίδια πηγή ίχνους 
παίρνουν ακριβώς αυτό που θέλουν και μόνο ό,τι θέλουν έξω από το σύστημα. Ούτε εσείς επηρεάζετε κάθε άλλο χρήστη 
αλλάζοντας ποιά πληροφορία είναι έξοδος από το σύστημα. Αν συμβεί να προσθέσετε μια πηγή ίχνους, το έργο σας ως καλός 
πολίτης ανοικτού κώδικα μπορεί να επιτρέψει σε άλλους χρήστες για την παροχή νέων υπηρεσιών κοινής ωφελείας που είναι ίσως 
πολύ χρήσιμο συνολικά, χωρίς να κάνει οποιεσδήποτε αλλαγές στον πυρήνα |ns3|.

..
	Simple Example

Απλό Παράδειγμα
+++++++++++++++

..
	Let's take a few minutes and walk through a simple tracing example.
	We are going to need a little background on Callbacks to understand
	what is happening in the example, so we have to take a small detour
	right away.

Ας πάρουμε μερικά λεπτά και βήμα βήμα ακολουθήστε ένα απλό παράδειγμα εντοπισμού. Θα χρειαστείτε την Επανάκληση 
να καταλάβετε τι συμβαίνει στο παράδειγμα, οπότε πρέπει να πάρετε μια μικρή παράκαμψη αμέσως.

..
	Callbacks

Επανάκληση
~~~~~~~~~~

..
	The goal of the Callback system in |ns3| is to allow one piece of code
	to call a function (or method in C++) without any specific
	inter-module dependency.  This ultimately means you need some kind of
	indirection -- you treat the address of the called function as a
	variable.  This variable is called a pointer-to-function variable.
	The relationship between function and pointer-to-function is
	really no different that that of object and pointer-to-object.
	
Ο στόχος του συστήματος επανάκλησης |ns3| είναι να επιτρέψει σε ένα κομμάτι του κώδικα να καλέσει μια συνάρτηση 
(ή μέθοδο σε C++), χωρίς καμία συγκεκριμένη μεταξύ των μονάδων εξάρτηση. Αυτό σημαίνει ότι, τελικά, θα πρέπει να 
έχετε κάποιο είδος εμμεσότητας -- αντιμετωπίζεις τη διεύθυνση της κληθήσας συνάρτησης ως μια μεταβλητή. Η 
μεταβλητή αυτή ονομάζεται μεταβλητή(pointer-to-function). Η σχέση μεταξύ της συνάρτησης και της μεταβλητής(pointer-to-function) 
πραγματικά δεν διαφέρει από αυτήν του αντικειμένου(object) και του δείκτη προς το αντικείμενο(pointer-to-object).

..
	In C the canonical example of a pointer-to-function is a
	pointer-to-function-returning-integer (PFI).  For a PFI taking one ``int``
	parameter, this could be declared like,

Στη C, το κανονικό παράδειγμα της μεταβλητής(pointer-to-function) δείκτη-συνάρτησης είναι ένας
δείκτης-σε-συνάρτηση-επιστρέφοντας-ακέραιο (PFI - pointer-to-function-returning-integer). Λαμβάνοντας μία παράμετρο ``int``, 
όπως,
::

  int (*pfi)(int arg) = 0;

..
	(But read the `C++-FAQ Section 33
	<http://www.parashift.com/c++-faq/pointers-to-members.html>`_ before
	writing code like this!)  What you get from this is a variable named
	simply ``pfi`` that is initialized to the value 0.  If you want to
	initialize this pointer to something meaningful, you have to have a
	function with a matching signature.  In this case, you could provide a
	function that looks like

(Αλλά διαβάστε το `C++-FAQ Section 33 <http://www.parashift.com/c++-faq/pointers-to-members.html>`_ πριν 
συντάξετε κώδικα σαν αυτόν!) Αυτό που μπορείτε να πάρετε από αυτό είναι μια μεταβλητή που ονομάζεται απλά ``pfi`` που έχει 
την τιμή 0. Αν θέλετε να αρχικοποιήσετε αυτό το δείκτη σε κάτι σημαντικό, θα πρέπει να έχετε μια συνάρτηση 
με μια υπογραφή που να ταιριάζουν. Σε αυτήν την περίπτωση, θα μπορείτε να προσφέρετε μια συνάρτηση που μοιάζει με
::

  int MyFunction (int arg) {}

..
	If you have this target, you can initialize the variable to point to
	your function
	
Εάν έχετε αυτό το στόχο, μπορείτε να προετοιμάσετε τη μεταβλητή στο σημείο της συνάρτησή σας
::

  pfi = MyFunction;

..
	You can then call MyFunction indirectly using the more suggestive form
	of the call

Στη συνέχεια μπορείτε να καλέσετε την MyFunction έμμεσα χρησιμοποιώντας την πιο υποβλητική μορφή της κλήσης
::

  int result = (*pfi) (1234);

..
	This is suggestive since it looks like you are dereferencing the
	function pointer just like you would dereference any pointer.
	Typically, however, people take advantage of the fact that the
	compiler knows what is going on and will just use a shorter form

Αυτό είναι ενδεικτικό, απο τη στιγμη που διαφοροποιήστε στο δείκτη συνάρτησης ακριβώς όπως θα κάνατε την διαφορά στον 
κάθε δείκτη. Συνήθως, όμως, οι άνθρωποι θα επωφεληθούν από το γεγονός ότι ο μεταγλωττιστής(compiler) ξέρει τι συμβαίνει και 
θα χρησιμοποιήσει μόνο μια μικρότερη μορφή
::

  int result = pfi (1234);

..
	This looks like you are calling a function named ``pfi``, but the
	compiler is smart enough to know to call through the variable ``pfi``
	indirectly to the function ``MyFunction``.

Αυτό μοιάζει σαν να καλείτε μια συνάρτηση που ονομάζεται ``pfi``, αλλά ο μεταγλωττιστής(compiler) είναι αρκετά έξυπνος για να ξέρει να 
καλέσει μέσω της μεταβλητής ``pfi`` έμμεσα τη συνάρτηση ``MyFunction``.

..
	Conceptually, this is almost exactly how the tracing system works.
	Basically, a trace sink *is* a callback.  When a trace sink expresses
	interest in receiving trace events, it adds itself as a Callback to a
	list of Callbacks internally held by the trace source.  When an
	interesting event happens, the trace source invokes its
	``operator(...)`` providing zero or more arguments.  The
	``operator(...)`` eventually wanders down into the system and does
	something remarkably like the indirect call you just saw, providing
	zero or more parameters, just as the call to ``pfi`` above passed one
	parameter to the target function ``MyFunction``.

Θεωρητικά, αυτό είναι σχεδόν ακριβώς πώς λειτουργεί το σύστημα εντοπισμού. Βασικά, ένα ίχνος καταβόθρας *είναι* μια 
επανάκληση. Όταν ένα ίχνος καταβόθρας(trace sink) εκφράζει ενδιαφέρον λαμβάνοντας γεγονότα ίχνους, η ίδια προσθέτει ως επανάκληση σε έναν 
κατάλογο Επανακλήσεων εσωτερικά διατηρημένα από την πηγή ίχνους. Όταν μια ενδιαφέρουσα εκδήλωση συμβαίνει, η πηγή 
ίχνους επικαλείται τον χειριστή της ``operator(...)`` παρέχοντας μηδέν ή περισσότερα ορίσματα. Ο χειριστής ``operator(...)`` περιπλανιέται 
τελικά κάτω στο σύστημα και κάνει κάτι σημαντικό όπως η έμμεση κλήση που μόλις είδατε, παρέχοντας μηδέν ή περισσότερες 
παραμέτρους, έτσι όπως ακριβώς και η κλήση για ``pfi`` παραπάνω περάσει μία παράμετρο για την συνάρτηση στόχο ``MyFunction``.

..
	The important difference that the tracing system adds is that for each
	trace source there is an internal list of Callbacks.  Instead of just
	making one indirect call, a trace source may invoke multiple
	Callbacks.  When a trace sink expresses interest in notifications from
	a trace source, it basically just arranges to add its own function to
	the callback list.

Η σημαντική διαφορά ότι το σύστημα εντοπισμού προσθέτει, είναι ότι για κάθε πηγή ίχνους υπάρχει μια εσωτερική λίστα 
Επανακλήσεων. Αντί να κάνουμε απλώς μια έμμεση κλήση, μια πηγή ίχνους μπορεί να καλέσει πολλές Επανακλήσεις. Όταν μία 
καταβόθρα ίχνους εκφράζει το ενδιαφέρον σε ειδοποιήσεις από μια πηγή ίχνους, ουσιαστικά φροντίζει μόνο να προσθέσει 
τη δική του συνάρτηση στη λίστα επανάκλησης.

..
	If you are interested in more details about how this is actually
	arranged in |ns3|, feel free to peruse the Callback section of the
	|ns3| Manual.

Εάν ενδιαφέρεστε για περισσότερες λεπτομέρειες σχετικά με το πώς είναι πραγματικά τοποθετημένα στον |ns3|, μη 
διστάσετε να μελετήσετε την ενότητα επανάκλησης του Εγχειριδίου(Manual) |ns3|.

..
	Walkthrough: ``fourth.cc``
	
Οδηγός Διασύνδεσης: ``fourth.cc``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
	We have provided some code to implement what is really the simplest
	example of tracing that can be assembled.  You can find this code in
	the tutorial directory as ``fourth.cc``.  Let's walk through it

Έχουμε προβάλει κάποιο κώδικα για να εφαρμόσουμε αυτό που είναι πραγματικά το πιο απλό παράδειγμα του εντοπισμού που 
μπορεί να συναρμολογηθεί. Μπορείτε να βρείτε τον κώδικα σε αυτό τον κατάλογο ως ``fourth.cc``. Ας δούμε μέσα 
από αυτό
::

  /* -*- Mode:C++; c-file-style:"gnu"; indent-tabs-mode:nil; -*- */
  /*
   * This program is free software; you can redistribute it and/or modify
   * it under the terms of the GNU General Public License version 2 as
   * published by the Free Software Foundation;
   *
   * This program is distributed in the hope that it will be useful,
   * but WITHOUT ANY WARRANTY; without even the implied warranty of
   * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
   * GNU General Public License for more details.
   *
   * You should have received a copy of the GNU General Public License
   * along with this program; if not, write to the Free Software
   * Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA  02111-1307  USA
   */
  
  #include "ns3/object.h"
  #include "ns3/uinteger.h"
  #include "ns3/traced-value.h"
  #include "ns3/trace-source-accessor.h"
  
  #include <iostream>
  
  using namespace ns3;

..
	Most of this code should be quite familiar to you.  As mentioned
	above, the trace system makes heavy use of the Object and Attribute
	systems, so you will need to include them.  The first two includes
	above bring in the declarations for those systems explicitly.  You
	could use the core module header to get everything at once, but we do
	the includes explicitly here to illustrate how simple this all really
	is.

Το μεγαλύτερο μέρος αυτού του κώδικα θα πρέπει να είναι αρκετά γνωστό σε εσάς. Όπως αναφέρθηκε παραπάνω, το σύστημα ίχνους 
κάνει βαριά χρήση του Αντικειμένου και του Χαρακτηριστικού στα συστήματα( Object and Attribute systems), έτσι θα πρέπει να τα 
συμπεριλάβετε. Τα δύο πρώτα περιεχόμενα φέρουν πάνω σε δηλώσεις για τα συστήματα αυτά ρητά. Θα μπορούσατε να 
χρησιμοποιήσετε τον πυρήνα κεφαλίδα ενότητας για να πάρετε τα πάντα με τη μία, αλλά κάνουμε τα περιεχόμενα ρητά εδώ για να 
επεξηγήσουμε πόσο πραγματικά απλό είναι αυτό όλο.

..
	The file, ``traced-value.h`` brings in the required declarations for
	tracing of data that obeys value semantics.  In general, value
	semantics just means that you can pass the object itself around,
	rather than passing the address of the object.  What this all really
	means is that you will be able to trace all changes made to a
	TracedValue in a really simple way.

Το αρχείο, ``traced-value.h`` φέρνει τις απαιτούμενες δηλώσεις για τον εντοπισμό των δεδομένων που υπακούει στη
σημασιολογική αξία. Σε γενικές γραμμές, η σημασιολογική αξία ακριβώς σημαίνει ότι μπορείτε να περάσετε το ίδιο το αντικείμενο 
γύρω, αντί να μεταθέσετε τη διεύθυνση του αντικειμένου. Αυτό σημαίνει πραγματικά ότι θα είστε σε θέση να εντοπίζετε 
όλες τις αλλαγές που γίνονται σε ένα TracedValue σε ένα πολύ απλό τρόπο.

..
	Since the tracing system is integrated with Attributes, and Attributes
	work with Objects, there must be an |ns3| ``Object`` for the trace
	source to live in.  The next code snippet declares and defines a
	simple Object we can work with.

Δεδομένου ότι το σύστημα ανίχνευσης είναι ενσωματωμένο με Χαρακτηριστικά, και τα Χαρακτηριστικά δουλεύουν με Αντικείμενα, 
πρέπει να υπάρχει ένας |ns3| ``Object`` για την πηγή ίχνους που υπάρχει. Το επόμενο απόσπασμα κώδικα δηλώνει και ορίζει ένα 
απλό Αντικείμενο που μπορούμε να εργαστούμε.
::

  class MyObject : public Object
  {
  public:
    static TypeId GetTypeId (void)
    {
      static TypeId tid = TypeId ("MyObject")
        .SetParent (Object::GetTypeId ())
        .AddConstructor<MyObject> ()
        .AddTraceSource ("MyInteger",
                         "An integer value to trace.",
                         MakeTraceSourceAccessor (&MyObject::m_myInt),
                         "ns3::Traced::Value::Int32Callback")
        ;
      return tid;
    }
    
    MyObject () {}
    TracedValue<int32_t> m_myInt;
  };

..
	The two important lines of code, above, with respect to tracing are
	the ``.AddTraceSource`` and the ``TracedValue`` declaration of
	``m_myInt``.

Οι δύο σημαντικές γραμμές κώδικα, παραπάνω, σε σχέση με τον εντοπισμό είναι το ``.AddTraceSource`` 
και η ``TracedValue`` δήλωση ``m_myInt``.

..
	The ``.AddTraceSource`` provides the "hooks" used for connecting the
	trace source to the outside world through the Config system.  The
	first argument is a name for this trace source, which makes it
	visible in the Config system. The second argument is a help string.
	Now look at the third argument, in fact focus on the *argument* of
	the third argument: ``&MyObject::m_myInt``. This is the TracedValue
	which is being added to the class; it is always a class data member.
	(The final argument is the name of a ``typedef`` for the TracedValue
	type, as a string.  This is used to generate documentation for the
	correct Callback function signature, which is useful especially
	for more general types of Callbacks.)

Το ``.AddTraceSource`` παρέχει τα "άγκιστρα" που χρησιμοποιούνται για τη σύνδεση της πηγής ίχνους με τον έξω κόσμο μέσω 
του συστήματος Config. Το πρώτο όρισμα είναι ένα όνομα για αυτήν την πηγή ίχνους, το οποίο το καθιστά ορατό στο σύστημα 
Config. Το δεύτερο όρισμα είναι μία βοήθεια απο string. Τώρα κοιτάξτε τον τρίτο όρισμα, στην πραγματικότητα εστίασε στο 
*όρισμα* από το τρίτο όρισμα: ``&MyObject::m_myInt``. Αυτή είναι η TracedValue η οποία προστίθεται στην κλάση(class), 
είναι πάντα ένα μέλος κλάσης δεδομένων. (Το τελευταίο όρισμα είναι το όνομα ``typedef`` για τον τύπο 
TracedValue, ως συμβολοσειρά(string). Αυτό χρησιμοποιείται για να δημιουργήσετε τεκμηρίωση για τη σωστή υπογραφή Επανάκλησης συνάρτησης, 
η οποία είναι χρήσιμη ειδικά για πιο γενικούς τύπους Επανακλήσεων.)
	
..
	The ``TracedValue<>`` declaration provides the infrastructure that
	drives the callback process.  Any time the underlying value is changed
	the TracedValue mechanism will provide both the old and the new value
	of that variable, in this case an ``int32_t`` value.  The trace
	sink function for this TracedValue will need the signature

Η δήλωση ``TracedValue<>`` παρέχει την υποδομή που οδηγεί την διαδικασία επανάκλησης. Κάθε φορά που η υποκείμενη αξία 
είναι αλλαγμένη ο μηχανισμός TracedValue θα παρέχει τόσο την παλαιά όσο και την νέα τιμή της μεταβλητής, σε αυτή την περίπτωση μία αξία 
``int32_t``. Η συνάρτηση της καταβόθρας ίχνους για το TracedValue θα χρειαστεί την υπογραφή
::

  void (* TracedValueCallback)(const int32_t oldValue, const int32_t newValue);

..
	All trace sinks hooking this trace source must have this signature.
	We'll discuss below how you can determine the required callback
	signature in other cases.

Όλες οι καταβόθρες ίχνους συνδέοντας αυτή την πηγή ίχνους πρέπει να έχουν αυτή την υπογραφή.
Θα συζητήσουμε παρακάτω πώς μπορείτε να προσδιορίσετε την απαιτούμενη υπογραφή επανάκλησης σε άλλες περιπτώσεις.

..
	Sure enough, continuing through ``fourth.cc`` we see

Συνεχίζοντας με το ``fourth.cc`` βλέπουμε
::

  void
  IntTrace (int32_t oldValue, int32_t newValue)
  {
    std::cout << "Traced " << oldValue << " to " << newValue << std::endl;
  }

..
	This is the definition of a matching trace sink.  It corresponds
	directly to the callback function signature.  Once it is connected,
	this function will be called whenever the ``TracedValue`` changes.

Αυτός είναι ο ορισμός μιάς καταβόθρας ίχνους. Αντιστοιχεί άμεσα με την υπογραφή της συνάρτησης επανάκλησης. 
Μόλις συνδεθεί, η συνάρτηση αυτή θα καλείται όταν το ``TracedValue`` αλλάζει.

..
	We have now seen the trace source and the trace sink.  What remains is
	code to connect the source to the sink, which happens in ``main``

Έχουμε δει τώρα την πηγή ίχνους και την καταβόθρα ίχνους. Αυτό που απομένει είναι ο κώδικας να συνδέσει την πηγή στην καταβόθρα, 
η οποία συμβαίνει στο ``main``
::

  int
  main (int argc, char *argv[])
  {
    Ptr<MyObject> myObject = CreateObject<MyObject> ();
    myObject->TraceConnectWithoutContext ("MyInteger", MakeCallback(&IntTrace));
  
    myObject->m_myInt = 1234;
  }

..
	Here we first create the MyObject instance in which the trace source
	lives.

Εδώ εμείς πρώτα δημιουργούμε το παράδειγμα MyObject στο οποίο η πηγή ίχνους υπάρχει.

..
	The next step, the ``TraceConnectWithoutContext``, forms the
	connection between the trace source and the trace sink.  The first
	argument is just the trace source name "MyInteger" we saw above.
	Notice the ``MakeCallback`` template function.  This function does the
	magic required to create the underlying |ns3| Callback object and
	associate it with the function ``IntTrace``.  ``TraceConnect`` makes
	the association between your provided function and overloaded
	``operator()`` in the traced variable referred to by the "MyInteger"
	Attribute.  After this association is made, the trace source will
	"fire" your provided callback function.

Το επόμενο βήμα, το ``TraceConnectWithoutContext``, αποτελεί τη σύνδεση μεταξύ της πηγής ίχνους και της καταβόθρας ίχνους. 
Το πρώτο όρισμα είναι ακριβώς το όνομα της πηγής ίχνους "MyInteger", είδαμε παραπάνω. Παρατήρησε την συνάρτηση πρότυπο ``MakeCallback``. 
Αυτή η συνάρτηση κάνει τη λειτουργία που απαιτείται για να δημιουργήσει το υποκείμενο Αντικείμενο επανάκλησης |ns3| 
και το συνδέουν με την συνάρτηση ``IntTrace``. Το ``TraceConnect`` κάνει την σχέση μεταξύ της παρεχόμενης συνάρτησής σας 
και υπερφορτωμένα ``operator()`` στην εντοπισμένη μεταβλητή που αναφέρεται από το Χαρακτηριστικό "MyInteger". 
Μετά από αυτή την ένωση, η πηγή ίχνους θα πάρει "φωτιά" στην παρεχόμενη συνάρτηση επανάκλησης.

..
	The code to make all of this happen is, of course, non-trivial, but
	the essence is that you are arranging for something that looks just
	like the ``pfi()`` example above to be called by the trace source.
	The declaration of the ``TracedValue<int32_t> m_myInt;`` in the Object
	itself performs the magic needed to provide the overloaded assignment
	operators that will use the ``operator()`` to actually invoke the
	Callback with the desired parameters.  The ``.AddTraceSource``
	performs the magic to connect the Callback to the Config system, and
	``TraceConnectWithoutContext`` performs the magic to connect your
	function to the trace source, which is specified by Attribute name.

Ο κώδικας για να κάνει όλα αυτά να συμβούν είναι, φυσικά, μη-τετριμμένο, αλλά η ουσία είναι ότι οργανώνετε για κάτι 
που μοιάζει ακριβώς όπως το παράδειγμα παραπάνω ``pfi()`` να κληθεί από την πηγή ίχνους. Η δήλωση του 
``TracedValue<int32_t> m_myInt;`` στο ίδιο το Αντικείμενο εκτελεί τη λειτουργία που απαιτείται για την παροχή των 
υπερφορτωμένων τελεστών ανάθεσης που θα χρησιμοποιήσει ο ``operator()`` για να επικαλεστεί πραγματικά την επανάκληση με τις 
επιθυμητές παραμέτρους. Το ``.AddTraceSource`` εκτελεί τη λειτουργία για να συνδέσετε την Επανάκληση στο σύστημα Config, και το 
`` TraceConnectWithoutContext`` εκτελεί τη λειτουργία για να συνδέσετε τη συνάρτησή σας με την πηγή ίχνους, η οποία καθορίζεται 
με βάση το όνομα του Χαρακτηριστικού.

..
	Let's ignore the bit about context for now.

Ας αγνοήσουμε για λίγο το κομμάτι σχετικά με το περιεχόμενο.

..
	Finally, the line assigning a value to ``m_myInt``

Τέλος, η γραμμή αποδίδοντας μία αξία σε ``m_myInt``
::

   myObject->m_myInt = 1234;

..
	should be interpreted as an invocation of ``operator=`` on the member
	variable ``m_myInt`` with the integer ``1234`` passed as a parameter.

θα πρέπει να ερμηνευθεί ως επίκληση του ``operator=`` για τη μεταβλητή μέλους ``m_myInt`` με τον ακέραιο ``1234`` 
πέρασε ως μία παράμετρος.

..
	Since ``m_myInt`` is a ``TracedValue``, this operator is defined to
	execute a callback that returns void and takes two integer values as
	parameters --- an old value and a new value for the integer in
	question.  That is exactly the function signature for the callback
	function we provided --- ``IntTrace``.

Από τη στιγμή που το ``m_myInt`` είναι ένα ``TracedValue``, ο φορέας αυτός ορίζεται να εκτελέσει μία επανάκληση που επιστρέφει 
κενό και παίρνει δύο ακέραιες τιμές ως παραμέτρους --- μια παλιά τιμή και μια νέα τιμή για τον εν λόγω ακέραιο. Αυτή 
είναι ακριβώς η υπογραφή συνάρτηση για την συνάρτηση επανάκλησης που παρείχαμε --- ``IntTrace``.

..
	To summarize, a trace source is, in essence, a variable that holds a
	list of callbacks.  A trace sink is a function used as the target of a
	callback.  The Attribute and object type information systems are used
	to provide a way to connect trace sources to trace sinks.  The act of
	"hitting" a trace source is executing an operator on the trace source
	which fires callbacks.  This results in the trace sink callbacks who
	registering interest in the source being called with the parameters
	provided by the source.
	
Για να συνοψίσουμε, μια πηγή ίχνους είναι, στην ουσία, μια μεταβλητή που κρατά μια λίστα επανακλήσεων(callbacks). Μία καταβόθρα ίχνους 
είναι μια συνάρτηση που χρησιμοποιείται ως στόχος της επανάκλησης. Τα συστήματα πληροφόρησης τύπου Χαρακτηριστικό και Αντικείμενο 
χρησιμοποιούνται για να παρέχουν έναν τρόπο για να συνδέσετε πηγές ίχνους για τον εντοπισμό καταβόθρων. Η ενέργεια 
"χτυπήματος" μιάς πηγής ίχνους εκτελείται σε ένα φορέα στην πηγή ίχνους που εκτοξεύει επανακλήσεις. Αυτά τα
αποτελέσματα επανακλήσεων στη καταβόθρα ίχνους τα οποία καταχωρούν ενδιαφέρον στην πηγή καλούνται με τις παραμέτρους που 
παρέχονται από την πηγή.

..
	If you now build and run this example,

Αν τώρα οικοδομήσουμε και να τρέξουμε αυτό το παράδειγμα,

.. sourcecode:: bash

  $ ./waf --run fourth

..
	you will see the output from the ``IntTrace`` function execute as soon
	as the trace source is hit
	
θα δείτε την έξοδο από την συνάρτηση ``IntTrace`` να εκτελεί το συντομότερο δυνατό
η πηγή ίχνους χτύπημα:

.. sourcecode:: bash

  Traced 0 to 1234

..
	When we executed the code, ``myObject->m_myInt = 1234;``, the trace
	source fired and automatically provided the before and after values to
	the trace sink.  The function ``IntTrace`` then printed this to the
	standard output.

Όταν έχουμε την εκτέλεση του κώδικα, ``myObject->m_myInt = 1234;``, η πηγή ίχνους εκτελείται γρήγορα και παρέχει αυτόματα 
τις τιμές πριν και μετά στη καταβόθρα ίχνους. Η συνάρτηση ``IntTrace`` στη συνέχεια εκτύπωσε αυτό στην κανονική έξοδο.

.. _Config:

..
	Connect with Config
	
Σύνδεση με Config
+++++++++++++++++

..
	The ``TraceConnectWithoutContext`` call shown above in the simple
	example is actually very rarely used in the system.  More typically,
	the ``Config`` subsystem is used to select a trace source in the
	system using what is called a *Config path*.  We saw an example of
	this in the previous section where we hooked the "CourseChange" event
	when we were experimenting with ``third.cc``.

Η κλήση του ``TraceConnectWithoutContext`` φαίνεται στο παραπάνω απλό παράδειγμα το οποίο χρησιμοποιείται στην πραγματικότητα πολύ σπάνια 
στο σύστημα. Πιο τυπικά, το υποσύστημα ``Config`` χρησιμοποιείται για να επιλέξετε μια πηγή ίχνους στο σύστημα, χρησιμοποιώντας 
αυτό που ονομάζεται *Config path*. Είδαμε ένα παράδειγμα απο αυτό στην προηγούμενη ενότητα, όπου γαντζώθηκε στην 
"CourseChange" εκδήλωση, όταν πειραματιζόμασταν με το ``third.cc``.

..
	Recall that we defined a trace sink to print course change information
	from the mobility models of our simulation.  It should now be a lot
	more clear to you what this function is doing

Υπενθυμίζουμε ότι ορίσαμε μία καταβόθρα ίχνους για να εκτυπώσουμε πληροφορίες και να αλλάξει πληροφορίες απο την κινητικότητα 
των μοντέλων απο την προσομοίωσή μας. Θα πρέπει τώρα να είναι πολύ πιο σαφές σε σας τι κάνει αυτή η συνάρτηση
::

  void
  CourseChange (std::string context, Ptr<const MobilityModel> model)
  {
    Vector position = model->GetPosition ();
    NS_LOG_UNCOND (context << 
      " x = " << position.x << ", y = " << position.y);
  }

..
	When we connected the "CourseChange" trace source to the above trace
	sink, we used a Config path to specify the source when we arranged a
	connection between the pre-defined trace source and the new trace
	sink

Όταν συνδέσαμε την πηγή ίχνους "CourseChange" στην παραπάνω καταβόθρα ίχνους, χρησιμοποιήσαμε ένα Config path για να 
προσδιορίσουμε την πηγή όταν κανονίσαμε μια σύνδεση μεταξύ της προ-καθορισμένης πηγής ίχνους και της νέας καταβόθρας ίχνους
::

  std::ostringstream oss;
  oss << "/NodeList/"
      << wifiStaNodes.Get (nWifi - 1)->GetId ()
      << "/$ns3::MobilityModel/CourseChange";

  Config::Connect (oss.str (), MakeCallback (&CourseChange));

..
	Let's try and make some sense of what is sometimes considered
	relatively mysterious code.  For the purposes of discussion, assume
	that the Node number returned by the ``GetId()`` is "7".  In this
	case, the path above turns out to be

Ας προσπαθήσουμε να καταλάβουμε του τι θεωρείται μερικές φορές σχετικά μυστηριώδης κώδικας. Για τους 
σκοπούς της συζήτησης, ας υποθέσουμε ότι ο αριθμός κόμβου(Node) που επιστρέφεται από το ``GetId ()`` είναι "7". Σε αυτήν την περίπτωση, 
η διαδρομή ανωτέρω αποδεικνύεται ότι είναι
::

  "/NodeList/7/$ns3::MobilityModel/CourseChange"

..
	The last segment of a config path must be an ``Attribute`` of an
	``Object``.  In fact, if you had a pointer to the ``Object`` that has
	the "CourseChange" ``Attribute`` handy, you could write this just like
	we did in the previous example.  You know by now that we typically
	store pointers to our ``Nodes`` in a NodeContainer.  In the ``third.cc``
	example, the Nodes of interest are stored in the ``wifiStaNodes``
	NodeContainer.  In fact, while putting the path together, we used this
	container to get a ``Ptr<Node>`` which we used to call ``GetId()``.  We
	could have used this ``Ptr<Node>`` to call a Connect method
	directly

Το τελευταίο τμήμα της διαδρομής config path πρέπει να είναι ``Attribute`` ενός ``Object``. Στην πραγματικότητα, αν είχατε πρακτικά 
ένα δείκτη στο ``Object`` που έχει το "CourseChange" ``Attribute``, θα μπορούσατε να γράψετε αυτό, ακριβώς όπως κάναμε στο 
προηγούμενο παράδειγμα. Τυπικά αποθηκεύουμε δείκτες στους (κόμβους) ``Nodes`` σε ένα NodeContainer. Στο παράδειγμα ``third.cc``, οι κόμβοι 
του ενδιαφέροντος είναι αποθηκευμένοι στο NodeContainer ``wifiStaNodes``. Στην πραγματικότητα, βάζοντας τη διαδρομή μαζί, 
χρησιμοποιήσαμε αυτό το container για να πάρει ένα ``Ptr<Node>`` που είχαμε συνηθίσει να λέμε ``GetId()``. Θα μπορούσαμε να 
χρησιμοποιήσουμε αυτό το ``Ptr<Node>`` να καλέσει άμεσα μια μέθοδο Σύνδεσης
::

  Ptr<Object> theObject = wifiStaNodes.Get (nWifi - 1);
  theObject->TraceConnectWithoutContext ("CourseChange", MakeCallback (&CourseChange));

..
	In the ``third.cc`` example, we actually wanted an additional "context"
	to be delivered along with the Callback parameters (which will be
	explained below) so we could actually use the following equivalent
	code

Στο παράδειγμα ``third.cc``, θέλαμε πραγματικά ένα πρόσθετο "περιεχόμενο" για να παραδοθεί μαζί με τις παραμέτρους Επανάκλησης 
(η οποία θα εξηγηθεί παρακάτω) έτσι θα μπορούσαμε να χρησιμοποιήσουμε πραγματικά τον ακόλουθο ισοδύναμο κώδικα
::

  Ptr<Object> theObject = wifiStaNodes.Get (nWifi - 1);
  theObject->TraceConnect ("CourseChange", MakeCallback (&CourseChange));

..
	It turns out that the internal code for
	``Config::ConnectWithoutContext`` and ``Config::Connect`` actually
	find a ``Ptr<Object>`` and call the appropriate ``TraceConnect``
	method at the lowest level.

Αυτό μας δίνει ότι ο εσωτερικός κώδικας για ``Config::ConnectWithoutContext`` και ``Config::Connect`` βρήκε πραγματικά μια 
``Ptr<Object>`` και κάλεσε την κατάλληλη μέθοδο ``TraceConnect`` στο χαμηλότερο επίπεδο .

..
	The ``Config`` functions take a path that represents a chain of
	``Object`` pointers.  Each segment of a path corresponds to an Object
	Attribute.  The last segment is the Attribute of interest, and prior
	segments must be typed to contain or find Objects.  The ``Config``
	code parses and "walks" this path until it gets to the final segment
	of the path.  It then interprets the last segment as an ``Attribute``
	on the last Object it found while walking the path.  The ``Config``
	functions then call the appropriate ``TraceConnect`` or
	``TraceConnectWithoutContext`` method on the final Object.  Let's see
	what happens in a bit more detail when the above path is walked.

Οι συναρτήσεις του ``Config`` λαμβάνουν μια διαδρομή που αντιπροσωπεύει μια αλυσίδα από δείκτες ``Object``. Κάθε τμήμα του μονοπατιού 
απαντάει σε ένα Χαρακτηριστικό Αντικείμενο. Το τελευταίο τμήμα είναι το Χαρακτηριστικό του ενδιαφέροντος, και πριν από τα 
τμήματα πρέπει να είναι τυπωμένα να περιέχουν ή να βρούν Αντικείμενα. Ο κώδικας ``Config`` αναλύει και "περπατάει" αυτό το 
μονοπάτι μέχρι να φτάσει στο τελικό τμήμα της διαδρομής. Στη συνέχεια ερμηνεύει το τελευταίο τμήμα ως ``Attribute`` στο 
τελευταίο Αντικείμενο που βρέθηκε, περπατώντας τη διαδρομή. Οι συναρτήσεις ``Config`` τότε καλούν την κατάλληλη 
``TraceConnect`` ή μέθοδο ``TraceConnectWithoutContext`` για το τελικό Αντικείμενο. Ας δούμε τι θα συμβεί σε λίγο 
με περισσότερες λεπτομέρειες, όταν η παραπάνω διαδρομή περπάτησε.

..
	The leading "/" character in the path refers to a so-called namespace.
	One of the predefined namespaces in the config system is "NodeList"
	which is a list of all of the nodes in the simulation.  Items in the
	list are referred to by indices into the list, so "/NodeList/7" refers
	to the eighth Node in the list of nodes created during the simulation
	(recall indices start at `0').  This reference is actually a
	``Ptr<Node>`` and so is a subclass of an ``ns3::Object``.

Η κορυφαία χαρακτήρας "/" στη διαδρομή αναφέρεται σε ένα λεγόμενο namespace. Μία από τις προκαθορισμένες namespaces στο 
σύστημα config είναι "NodeList", η οποία είναι μια λίστα με όλους τους κόμβους στην προσομοίωση. Τα στοιχεία στην λίστα 
αναφέρονται στους δείκτες της λίστας, έτσι "/ NodeList / 7" αναφέρεται στον όγδοο Κόμβο στη λίστα των κόμβων που δημιουργούνται 
κατά τη διάρκεια της προσομοίωσης (ανάκληση δεικτών ξεκινούν από το `0'). Αυτή η αναφορά είναι στην πραγματικότητα ένα 
``Ptr<Node>`` και έτσι είναι μια υποκατηγορία ενός ``ns3::Object``.

..
	As described in the Object Model section of the |ns3| Manual, we make
	widespread use of object aggregation.  This allows us to form an
	association between different Objects without building a complicated
	inheritance tree or predeciding what objects will be part of a
	Node.  Each Object in an Aggregation can be reached from the other
	Objects.

Όπως περιγράφεται στην ενότητα Object Model της |ns3| Εγχειρίδιο(Manual), κάνουμε εκτεταμένη χρήση της συνάθροισης αντικειμένου. 
Αυτό μας επιτρέπει να σχηματίσουμε τη σύνδεση μεταξύ διαφορετικών Αντικειμένων χωρίς την οικοδόμηση ενός δέντρου πολύπλοκης 
κληρονομικότητας ή να προ-αποφασίσουμε ποια αντικείμενα θα είναι μέρος ενός Κόμβου. Κάθε Αντικείμενο σε ένα σύνολο μπορεί να 
επιτευχθεί από τα άλλα Αντικείμενα.

..
	In our example the next path segment being walked begins with the "$"
	character.  This indicates to the config system that the segment is
	the name of an object type, so a ``GetObject`` call should be made
	looking for that type.  It turns out that the ``MobilityHelper`` used
	in ``third.cc`` arranges to Aggregate, or associate, a mobility model
	to each of the wireless ``Nodes``.  When you add the "$" you are
	asking for another Object that has presumably been previously
	aggregated.  You can think of this as switching pointers from the
	original Ptr<Node> as specified by "/NodeList/7" to its associated
	mobility model --- which is of type ``ns3::MobilityModel``.  If you
	are familiar with ``GetObject``, we have asked the system to do the
	following

Στο παράδειγμά μας, το επόμενο τμήμα της διαδρομής που έχει περπατηθεί ξεκινά με το χαρακτήρα "$". Αυτό δηλώνει στο σύστημα 
config ότι το τμήμα είναι το όνομα ενός τύπου Αντικειμένου, ώστε η κλήση ``GetObject`` θα πρέπει να ψάχνει για αυτό το είδος. 
Μας βγάζει ότι η ``MobilityHelper`` χρησιμοποιείται στο ``third.cc`` και κανονίζει στο σύνολο ή συνδέει, ένα μοντέλο 
κινητικότητας σε κάθε ασύρματο Κόμβο ``Nodes``. Όταν προσθέσετε το "$" ρωτάτε για ένα άλλο Αντικείμενο που έχει πιθανώς 
προηγουμένως αθροιστεί. Μπορείτε να σκεφτείτε αυτό ως εναλλαγή δεικτών από την αρχική Ptr<Node>, όπως καθορίζεται από το 
"/NodeList/7" στο ίδιο συνδεδεμένο μοντέλο κινητικότητάς του --- το οποίο είναι τύπου ``ns3::MobilityModel``. Εάν είστε 
εξοικειωμένοι με το ``GetObject``, ζητήσαμε από το σύστημα να κάνετε τα εξής
::

  Ptr<MobilityModel> mobilityModel = node->GetObject<MobilityModel> ()

..
	We are now at the last Object in the path, so we turn our attention to
	the Attributes of that Object.  The ``MobilityModel`` class defines an
	Attribute called "CourseChange".  You can see this by looking at the
	source code in ``src/mobility/model/mobility-model.cc`` and searching
	for "CourseChange" in your favorite editor.  You should find

Είμαστε τώρα στο τελευταίο Αντικείμενο στο μονοπάτι, έτσι ώστε να στρέψουμε την προσοχή μας προς τα Χαρακτηριστικά αυτού του 
Αντικειμένου. Η κλάση ``MobilityModel`` ορίζει ένα Χαρακτηριστικό που ονομάζεται "CourseChange". Μπορείτε να δείτε αυτό 
κοιτάζοντας τον πηγαίο κώδικα σε ``src/mobility/model/mobility-model.cc`` και αναζητώντας για "CourseChange" στο αγαπημένο 
σας επεξεργαστή(editor). Θα πρέπει να βρείτε
::

  .AddTraceSource ("CourseChange",
                   "The value of the position and/or velocity vector changed",
                   MakeTraceSourceAccessor (&MobilityModel::m_courseChangeTrace),
                   "ns3::MobilityModel::CourseChangeCallback")

..
	which should look very familiar at this point. 

το οποίο θα έπρεπε να φαίνεται εξοικειωμένο σε αυτό το σημείο.

..
	If you look for the corresponding declaration of the underlying traced
	variable in ``mobility-model.h`` you will find

Αν κοιτάξετε για την αντίστοιχη δήλωση της υποκείμενης εντοπισμένης μεταβλητής στο ``mobility-model.h`` θα βρείτε
::

  TracedCallback<Ptr<const MobilityModel> > m_courseChangeTrace;

..
	The type declaration ``TracedCallback`` identifies
	``m_courseChangeTrace`` as a special list of Callbacks that can be
	hooked using the Config functions described above.  The ``typedef``
	for the callback function signature is also defined in the header file

Ο τύπος της δήλωσης ``TracedCallback`` προσδιορίζει ``m_courseChangeTrace`` ως μία ειδική λίστα των Επανακλήσεων που μπορεί 
να συνδεθεί με τις συναρτήσεις Config που περιγράφονται παραπάνω. Το ``typedef`` για την υπογραφή συνάρτησης επανάκλησης είναι 
επίσης ορισμένο στο αρχείο κεφαλίδας
::

  typedef void (* CourseChangeCallback)(Ptr<const MobilityModel> * model);

..
	The ``MobilityModel`` class is designed to be a base class providing a
	common interface for all of the specific subclasses.  If you search
	down to the end of the file, you will see a method defined called
	``NotifyCourseChange()``

Η κλάση ``MobilityModel`` έχει σχεδιαστεί για να είναι μια βασική κλάση που παρέχει μια κοινή διεπαφή για όλες τις συγκεκριμένες 
υποκατηγορίες. Εάν κάνετε αναζήτηση προς τα κάτω στο τέλος του αρχείου, θα δείτε μια μέθοδο που λέγεται ``NotifyCourseChange()``
::

  void
  MobilityModel::NotifyCourseChange (void) const
  {
    m_courseChangeTrace(this);
  }

..
	Derived classes will call into this method whenever they do a course
	change to support tracing.  This method invokes ``operator()`` on the
	underlying ``m_courseChangeTrace``, which will, in turn, invoke all of
	the registered Callbacks, calling all of the trace sinks that have
	registered interest in the trace source by calling a Config function.

Οι παραγόμενες κλάσεις θα καλέσουν σε αυτή τη μέθοδο κάθε φορά που κάνουν μια αλλαγή πορείας για την υποστήριξη ανίχνευσης. 
Η μέθοδος αυτή επικαλείται τον ``operator()`` σχετικά με το υποκείμενο ``m_courseChangeTrace``, το οποίο, με τη σειρά του, 
επικαλείται το σύνολο των εγγεγραμμένων Επανακλήσεων, καλώντας όλες τις καταβόθρες ίχνους που έχουν εγγραφεί με ενδιαφέρον για 
την πηγή ίχνους καλώντας μία συνάρτηση Config.

..
	So, in the ``third.cc`` example we looked at, whenever a course change
	is made in one of the ``RandomWalk2dMobilityModel`` instances
	installed, there will be a ``NotifyCourseChange()`` call which calls
	up into the ``MobilityModel`` base class.  As seen above, this invokes
	``operator()`` on ``m_courseChangeTrace``, which in turn, calls any
	registered trace sinks.  In the example, the only code registering an
	interest was the code that provided the Config path.  Therefore, the
	``CourseChange`` function that was hooked from Node number seven will
	be the only Callback called.
	
Έτσι, στο παράδειγμα ``third.cc`` κοιτάξαμε, κάθε φορά που μια αλλαγή πορείας γίνεται σε μία απο τις εγκαταστημένες περιπτώσεις ``RandomWalk2dMobilityModel``, 
θα υπάρχει μία κλήση ``NotifyCourseChange()`` που καλεί επάνω σε μία κλάση βάσης `` MobilityModel``. Όπως είδαμε παραπάνω, 
αυτό επικαλείται ``operator()`` στη ``m_courseChangeTrace``, η οποία με τη σειρά της, καλεί οποιαδήποτε εγγεγραμμένη 
καταβόθρα ίχνους. Στο παράδειγμα, ο μοναδικός κώδικας, καταγράφοντας ένα ενδιαφέρον ήταν ο κώδικας που έδωσε το μονοπάτι 
Config. Ως εκ τούτου, η συνάρτηση ``CourseChange`` που γαντζώθηκε από τον αριθμό του κόμβου επτά θα είναι η μόνη που 
ονομάζεται Επανάκληση.

..
	The final piece of the puzzle is the "context".  Recall that we saw an
	output looking something like the following from ``third.cc``

Το τελευταίο κομμάτι του παζλ είναι το "περιεχόμενο". Υπενθυμίζουμε ότι είδαμε μια έξοδο που αναζητά κάτι σαν το παρακάτω 
από το ``third.cc``
::

  /NodeList/7/$ns3::MobilityModel/CourseChange x = 7.27897, y =
  2.22677

..
	The first part of the output is the context.  It is simply the path
	through which the config code located the trace source.  In the case
	we have been looking at there can be any number of trace sources in
	the system corresponding to any number of nodes with mobility models.
	There needs to be some way to identify which trace source is actually
	the one that fired the Callback.  The easy way is to connect with
	``Config::Connect``, instead of ``Config::ConnectWithoutContext``.

Το πρώτο μέρος της εξόδου είναι το περιεχόμενο. Είναι απλά η διαδρομή μέσω της οποίας ο κώδικας config βρίσκεται στη πηγή ίχνους. 
Στην περίπτωση που εμείς κοιτάζαμε ότι μπορεί να υπάρχει οποιοσδήποτε αριθμός των πηγών ίχνους στο σύστημα απαντά σε 
οποιονδήποτε αριθμό των κόμβων με τα μοντέλα κινητικότητας. Πρέπει να υπάρχει κάποιος τρόπος να προσδιορίσει ποια πηγή ίχνους 
είναι στην πραγματικότητα αυτός που έβαλε φωτιά στην Επανάκληση. Ο εύκολος τρόπος είναι να συνδέσετε με ``Config::Connect``, 
αντί για ``Config::ConnectWithoutContext``

..
	Finding Sources

Βρίσκοντας Πηγές
++++++++++++++++

..
	The first question that inevitably comes up for new users of the
	Tracing system is, *"Okay, I know that there must be trace sources in
	the simulation core, but how do I find out what trace sources are
	available to me?"*

Το πρώτο ερώτημα που έρχεται αναπόφευκτα για τους νέους χρήστες του συστήματος Ανίχνευσης είναι, *"Εντάξει, ξέρω ότι πρέπει 
να υπάρχουν πηγές ίχνους στον πυρήνα προσομοίωσης, αλλά πώς μπορώ να μάθω τι ίχνους πηγές είναι διαθέσιμες για εμένα;"*

..
	The second question is, *"Okay, I found a trace source, how do I figure
	out the Config path to use when I connect to it?"*

Το δεύτερο ερώτημα είναι, *"Εντάξει, βρήκα μια πηγή ίχνους, πώς μπορώ να καταλάβω το μονοπάτι Config για να χρησιμοποιήσω 
όταν συνδεθώ σε αυτό;"*

..
	The third question is, *"Okay, I found a trace source and the Config
	path, how do I figure out what the return type and formal arguments of
	my callback function need to be?"*

Το τρίτο ερώτημα είναι, *"Εντάξει, βρήκα μια πηγή ίχνους και το μονοπάτι Config, πώς μπορώ να καταλάβω ποιο είναι το είδος 
επιστροφής και ποια πρέπει να είναι τα επίσημα ορίσματα της συνάρτησης επανάκλησής μου;"*

..
	The fourth question is, *"Okay, I typed that all in and got this
	incredibly bizarre error message, what in the world does it mean?"*

Το τέταρτο ερώτημα είναι, *"Εντάξει, εγώ τα πληκτρολόγησα όλα σωστά και πήρα αυτό το απίστευτα παράξενο μήνυμα σφάλματος, τι 
στον κόσμο σημαίνει αυτό;"*

..
	We'll address each of these in turn.

Θα τις απαντήσουμε κάθε μια από αυτές με τη σειρά.

..
	Available Sources

Διαθέσιμες Πηγές
++++++++++++++++

..
	*Okay, I know that there must be trace sources in the simulation core,
	but how do I find out what trace sources are available to me?*

*Εντάξει, ξέρω ότι πρέπει να υπάρχουν πηγές ίχνους στον πυρήνα προσομοίωσης, αλλά πώς μπορώ να μάθω τι πηγές ίχνους είναι 
διαθέσιμες για εμένα;*

..
	The answer to the first question is found in the |ns3| API
	documentation.  If you go to the project web site, `ns-3 project
	<http://www.nsnam.org>`_, you will find a link to "Documentation" in
	the navigation bar.  If you select this link, you will be taken to our
	documentation page. There is a link to "Latest Release" that will take
	you to the documentation for the latest stable release of |ns3|.  If
	you select the "API Documentation" link, you will be taken to the
	|ns3| API documentation page.

Η απάντηση στο πρώτο ερώτημα βρίσκεται στην τεκμηρίωση |ns3| API. Αν πάτε στην ιστοσελίδα του έργου, `ns-3 project 
<http://www.nsnam.org>`_, θα βρείτε ένα σύνδεσμο στο "Documentation" στη γραμμή πλοήγησης. Αν επιλέξετε αυτό το σύνδεσμο, 
θα μεταφερθείτε στη σελίδα τεκμηρίωσης. Υπάρχει μια σύνδεση με τα "Latest Release" που θα σας μεταφέρει στην τεκμηρίωση για 
την τελευταία σταθερή έκδοση του |ns3|. Εάν επιλέξετε το σύνδεσμο "API Documentation", θα πρέπει να ληφθούν για την |ns3| 
API σελίδα τεκμηρίωσης.

..
	In the sidebar you should see a hierachy that begins

Στην πλαϊνή γραμμή θα πρέπει να δείτε μια ιεραρχία που αρχίζει

*  ns-3

  *  ns-3 Documentation
  *  All TraceSources
  *  All Attributes
  *  All GlobalValues

..
	The list of interest to us here is "All TraceSources".  Go ahead and
	select that link.  You will see, perhaps not too surprisingly, a list
	of all of the trace sources available in |ns3|.

Η λίστα που μας ενδιαφέρει είναι "All TraceSources". Προχωρήστε και επιλέξτε το σύνδεσμο. Θα δείτε, ίσως όχι 
και τόσο έκπληκτα, μια λίστα με όλες τις πηγές ίχνους διαθέσιμο στον |ns3|.

..
	As an example, scroll down to ``ns3::MobilityModel``.  You will find
	an entry for

Ως παράδειγμα, μετακινηθείτε προς τα κάτω για να ``ns3::MobilityModel``. Θα βρείτε μια καταχώρηση για
::

  CourseChange: The value of the position and/or velocity vector changed 

..
	You should recognize this as the trace source we used in the
	``third.cc`` example.  Perusing this list will be helpful.
	
Θα πρέπει να αναγνωρίσουμε αυτό ως πηγή ίχνους που χρησιμοποιείται στο παράδειγμα ``third.cc``. Θα είναι χρήσιμη η 
περιεργασία της λίστας.

..
	Config Paths

Μονοπάτια Config
++++++++++++++++

..
	*Okay, I found a trace source, how do I figure out the Config path to
	use when I connect to it?*

*Εντάξει, βρήκα μια πηγή ίχνους, πώς μπορώ να καταλάβω την πορεία Config για να την χρησιμοποιήσω όταν συνδεθώ σε αυτή;*

..
	If you know which object you are interested in, the "Detailed
	Description" section for the class will list all available trace
	sources.  For example, starting from the list of "All TraceSources,"
	click on the ``ns3::MobilityModel`` link, which will take you to the
	documentation for the ``MobilityModel`` class.  Almost at the top of
	the page is a one line brief description of the class, ending in a
	link "More...".  Click on this link to skip the API summary and go to
	the "Detailed Description" of the class.  At the end of the
	description will be (up to) three lists:

Εάν γνωρίζετε ποιο αντικείμενο σας ενδιαφέρει, το τμήμα "Detailed Description" για την κλάση θα εμφανίσει όλες τις διαθέσιμες 
πηγές ίχνους. Για παράδειγμα, ξεκινώντας από τη λίστα των "All TraceSources", κάντε κλικ στο σύνδεσμο ``ns3::MobilityModel``, 
το οποίο θα σας μεταφέρει στην τεκμηρίωση για την κλάση ``MobilityModel``. Σχεδόν στην κορυφή της σελίδας είναι μία γραμμή 
σύντομης περιγραφής της κλάσης, που καταλήγει σε ένα σύνδεσμο "More...". Κάντε κλικ σε αυτό το σύνδεσμο για να παρακάμψετε 
την περίληψη API και πηγαίνετε στο "Detailed Description" της κλάσης. Στο τέλος της περιγραφής, θα είναι (έως) τρεις λίστες:

..
	* **Config Paths**: a list of typical Config paths for this class.
	* **Attributes**: a list of all attributes supplied by this class.
	* **TraceSources**: a list of all TraceSources available from this class.

* **Config Paths**: μια λίστα των τυπικών διαδρομών Config για αυτή την κλάση.
* **Attributes** μια λίστα με όλα τα χαρακτηριστικά που παρέχονται από αυτή την κλάση.
* **TraceSources**: μια λίστα με όλα τα διαθέσιμα TraceSources από αυτή την κλάση.

..
	First we'll discuss the Config paths.

Πρώτα θα συζητήσουμε τις διαδρομές Config.

..
	Let's assume that you have just found the "CourseChange" trace source
	in the "All TraceSources" list and you want to figure out how to
	connect to it.  You know that you are using (again, from the
	``third.cc`` example) an ``ns3::RandomWalk2dMobilityModel``.  So
	either click on the class name in the "All TraceSources" list, or find
	``ns3::RandomWalk2dMobilityModel`` in the "Class List".  Either way 
	you should now be looking at the "ns3::RandomWalk2dMobilityModel Class
	Reference" page.

Ας υποθέσουμε ότι έχετε μόλις βρεί την πηγή ίχνους "CourseChange" στη λίστα "All TraceSources" και θέλετε να βρείτε πώς να 
συνδεθείτε με αυτή. Ξέρετε ότι χρησιμοποιείτε (και πάλι, από το παράδειγμα ``third.cc``) ένα ``ns3::RandomWalk2dMobilityModel``. 
Έτσι, είτε να κάνετε κλικ στο όνομα της κλάσης στη λίστα "All TraceSources", ή να βρείτε ``ns3::RandomWalk2dMobilityModel`` 
στην "Class List". Είτε έτσι είτε αλλιώς θα πρέπει τώρα να εξετάσουμε τη σελίδα "ns3::RandomWalk2dMobilityModel Class Reference".

..
	If you now scroll down to the "Detailed Description" section, after
	the summary list of class methods and attributes (or just click on the
	"More..." link at the end of the class brief description at the top of
	the page) you will see the overall documentation for the class.
	Continuing to scroll down, find the "Config Paths" list:

Αν τώρα μετακινηθείτε προς τα κάτω στην ενότητα "Detailed Description", μετά από τον ενοποιημένη λίστα των μεθόδων και χαρακτηριστικών 
κλάσης (ή απλά κάντε κλικ στο σύνδεσμο "More..." στο τέλος της κλάσης σύντομη περιγραφή στο πάνω μέρος της σελίδας) θα δείτε 
τη συνολική τεκμηρίωση για την κλάση. Συνεχίζοντας προς τα κάτω, βρείτε τη λίστα "Config Paths":

  **Config Paths**

  ``ns3::RandomWalk2dMobilityModel`` is accessible through the
  following paths with ``Config::Set`` and ``Config::Connect``:

  * "/NodeList/[i]/$ns3::MobilityModel/$ns3::RandomWalk2dMobilityModel"

..
	The documentation tells you how to get to the
	``RandomWalk2dMobilityModel`` Object.  Compare the string above with
	the string we actually used in the example code
	
Η τεκμηρίωση σας λέει πώς να φτάσετε στο Αντικείμενο ``RandomWalk2dMobilityModel``. Συγκρίνετε τη σειρά πάνω από τη σειρά 
που πράγματι χρησιμοποιήσαμε στο παράδειγμα κώδικα
::

  "/NodeList/7/$ns3::MobilityModel"

..
	The difference is due to the fact that two ``GetObject`` calls are
	implied in the string found in the documentation.  The first, for
	``$ns3::MobilityModel`` will query the aggregation for the base class.
	The second implied ``GetObject`` call, for
	``$ns3::RandomWalk2dMobilityModel``, is used to cast the base class to
	the concrete implementation class.  The documentation shows both of
	these operations for you.  It turns out that the actual trace source
	you are looking for is found in the base class.

Η διαφορά αυτή οφείλεται στο γεγονός ότι οι δύο κλήσεις ``GetObject`` υπονοούν στη σειρά που βρίσκονται στην τεκμηρίωση. Η πρώτη, 
για ``$ns3::MobilityModel`` θα ζητήσει το σύνολα για την βάση της κλάσης. Η δεύτερη κλήση ``GetObject``, για 
``$ns3::RandomWalk2dMobilityModel``, χρησιμοποιείται για να ρίχνει την βάση της κλάσης για την συγκεκριμένη κλάση εφαρμογής. 
Η τεκμηρίωση δείχνει τις δύο αυτές λειτουργίες για εσάς. Αποδεικνύεται ότι η πραγματική πηγή ίχνους που ψάχνετε βρίσκεται 
στη βάση της κλάσης.

..
	Look further down in the "Detailed Description" section for the list
	of trace sources.  You will find

Δείτε πιο κάτω στην ενότητα "Detailed Description" για τη λίστα των πηγών ίχνους. Θα βρείτε

  No TraceSources are defined for this type.
  
  **TraceSources defined in parent class ``ns3::MobilityModel``**

  * **CourseChange**: The value of the position and/or velocity vector
    changed.

    Callback signature: ``ns3::MobilityModel::CourseChangeCallback``

..
	This is exactly what you need to know.  The trace source of interest
	is found in ``ns3::MobilityModel`` (which you knew anyway).  The
	interesting thing this bit of API Documentation tells you is that you
	don't need that extra cast in the config path above to get to the
	concrete class, since the trace source is actually in the base class.
	Therefore the additional ``GetObject`` is not required and you simply
	use the path
	
Αυτό είναι ακριβώς ό,τι χρειάζεται να ξέρετε. Η πηγή ίχνους του ενδιαφέροντος βρίσκεται στο ``ns3::MobilityModel`` (που 
ξέρατε ούτως ή άλλως). Το ενδιαφέρον πράγμα σε αυτό το κομμάτι της τεκμηρίωσης του API λέει ότι δεν χρειάζονται επιπλέον 
απορρίμματα στο μονοπάτι config παραπάνω για να φτάσουμε στην συγκεκριμένη κλάση, δεδομένου ότι η πηγή ίχνους είναι στην 
πραγματικότητα στην βάση της κλάσης. Ως εκ τούτου, η πρόσθετη ``GetObject`` δεν απαιτείται και μπορείτε απλά να 
χρησιμοποιήσετε τη διαδρομή
::

  "/NodeList/[i]/$ns3::MobilityModel"

..
	which perfectly matches the example path

που ταιριάζει απόλυτα με το παράδειγμα διαδρομή
::

  "/NodeList/7/$ns3::MobilityModel"

..
	As an aside, another way to find the Config path is to ``grep`` around in
	the |ns3| codebase for someone who has already figured it out.  You
	should always try to copy someone else's working code before you start
	to write your own.  Try something like:

Παρεμπιπτόντως, ένας άλλος τρόπος για να βρείτε το μονοπάτι Config είναι το ``grep`` γύρω στον κώδικα βάσης |ns3| για κάποιον 
που έχει ήδη καταλάβει. Πρέπει πάντα να προσπαθείτε να αντιγράψετε τον κώδικα εργασίας κάποιου άλλου πριν ξεκινήσετε να 
γράφετε τα δικά σας. Δοκιμάστε κάτι σαν:

.. sourcecode:: bash

  $ find . -name '*.cc' | xargs grep CourseChange | grep Connect

..
	and you may find your answer along with working code.  For example, in
	this case, ``src/mobility/examples/main-random-topology.cc`` has
	something just waiting for you to use

και μπορείτε να βρείτε την απάντησή σας, μαζί με τον κώδικα εργασίας. Για παράδειγμα, στην περίπτωση αυτή, 
``src/mobility/examples/main-random-topology.cc`` έχει κάτι που σας περιμένει να χρησιμοποιήσετε 
::

  Config::Connect ("/NodeList/*/$ns3::MobilityModel/CourseChange", 
    MakeCallback (&CourseChange));

..
	We'll return to this example in a moment.    

Θα επανέλθω σε αυτό το παράδειγμα σε λίγο.
      
..
	Callback Signatures

Στίγματα Επανάκλησης
+++++++++++++++++++++++

..
	*Okay, I found a trace source and the Config path, how do I figure out
	what the return type and formal arguments of my callback function need
	to be?*

*Εντάξει, βρήκα μια πηγή ίχνους και το μονοπάτι Config, πώς μπορώ να υπολογίσω ποιος είναι ο τύπος επιστροφής και τα επίσημα 
ορίσματα της συνάρτησης επανάκλησης μου; *

..
	The easiest way is to examine the callback signature ``typedef``,
	which is given in the "Callback signature" of the trace source in the
	"Detailed Description" for the class, as shown above.

Ο ευκολότερος τρόπος είναι να εξετάσει το `` typedef`` υπογραφή επανάκλησης, το οποίο δίνεται στο "Τηλεφωνική υπογραφή» της πηγής ίχνος στο "Λεπτομερής Περιγραφή" της κατηγορίας, όπως φαίνεται παραπάνω.

..
	Repeating the "CourseChange" trace source entry from
	``ns3::RandomWalk2dMobilityModel`` we have:
	
Η επανάληψη της "CourseChange" καταχώρηση πηγή ίχνος από `` NS3 :: RandomWalk2dMobilityModel`` έχουμε:

  * **CourseChange**: The value of the position and/or velocity vector
    changed.

    Callback signature: ``ns3::MobilityModel::CourseChangeCallback``

..
	The callback signature is given as a link to the relevant ``typedef``,
	where we find

Η υπογραφή επανάκλησης δίνεται ως ένα σύνδεσμο με τη σχετική `` typedef``, όπου βρίσκουμε

  ``typedef void (* CourseChangeCallback)(const std::string context, Ptr<const MobilityModel> * model);``
					  
  **TracedCallback** signature for course change notifications.

  ..If the callback is connected using ``ConnectWithoutContext`` omit the
  ``context`` argument from the signature.
  
  Αν ο επανάκλησης συνδέεται με τη χρήση `` ConnectWithoutContext`` παραλείψτε το `` context`` επιχείρημα από την υπογραφή.

  **Parameters**:

      |  [in] :param:`context` The context string supplied by the Trace source.
      |  [in] :param:`model` The MobilityModel which is changing course.

..
	As above, to see this in use ``grep`` around in the |ns3| codebase for
	an example.  The example above, from
	``src/mobility/examples/main-random-topology.cc``, connects the
	"CourseChange" trace source to the ``CourseChange`` function in the
	same file

Όπως και παραπάνω, για να δείτε αυτό κατά τη χρήση `` grep`` γύρω στο | NS3 | codebase για παράδειγμα. Το παραπάνω παράδειγμα, από `` src / κινητικότητα / παραδείγματα / κύρια-τυχαία-topology.cc``, συνδέει την «CourseChange" πηγή ίχνος στην `` λειτουργία CourseChange`` στο ίδιο αρχείο
::

  static void
  CourseChange (std::string context, Ptr<const MobilityModel> model)
  {
    ...
  }

..
	Notice that this function:

Σημειώστε ότι αυτή η λειτουργία:

..* Takes a "context" string argument, which we'll describe in a minute.
  (If the callback is connected using the ``ConnectWithoutContext``
  function the ``context`` argument will be omitted.)
  
  * Παίρνει ένα όρισμα "πλαίσιο", το οποίο θα περιγράψουμε σε ένα λεπτό.
(Αν ο επανάκλησης συνδέεται με τη χρήση του `` ConnectWithoutContext`` λειτουργήσει το `` context`` επιχείρημα θα πρέπει 
να παραλείπεται.)

  
..* Has the ``MobilityModel`` supplied as the last argument (or only
  argument if ``ConnectWithoutContext`` is used).
  
  * Έχει την `` MobilityModel`` παρέχεται ως το τελευταίο επιχείρημα (ή μόνο επιχείρημα εάν `` ConnectWithoutContext`` 
  χρησιμοποιείται).

..* Returns ``void``.

* Επιστροφές `` void``.

..
	If, by chance, the callback signature hasn't been documented, and
	there are no examples to work from, determining the right callback
	function signature can be, well, challenging to actually figure out
	from the source code.

Αν, κατά τύχη, η υπογραφή επανάκλησης δεν έχει τεκμηριωθεί, και δεν υπάρχουν παραδείγματα για να εργαστούν από, τον 
προσδιορισμό του δικαιώματος υπογραφής επανάκλησης λειτουργία μπορεί να είναι, επίσης, δύσκολο να πραγματικά να καταλάβω 
από τον πηγαίο κώδικα.

..
	Before embarking on a walkthrough of the code, I'll be kind and just
	tell you a simple way to figure this out: The return value of your
	callback will always be ``void``.  The formal parameter list for a
	``TracedCallback`` can be found from the template parameter list in
	the declaration.  Recall that for our current example, this is in
	``mobility-model.h``, where we have previously found

Πριν να προβούν σε μια περιδιάβαση του κώδικα, θα είμαι το είδος και απλά να σας πω έναν απλό τρόπο για να το καταλάβω: Η τιμή επιστροφής της επανάκλησης σας θα είναι πάντα `` void``. Η επίσημη λίστα παραμέτρων για μια `` TracedCallback`` μπορεί να βρεθεί από τη λίστα παράμετρο προτύπου στη δήλωση. Υπενθυμίζεται ότι για το τρέχον παράδειγμα μας, αυτό είναι `` κινητικότητα-model.h``, όπου βρήκαμε προηγουμένως
::

  TracedCallback<Ptr<const MobilityModel> > m_courseChangeTrace;

..
	There is a one-to-one correspondence between the template parameter
	list in the declaration and the formal arguments of the callback
	function.  Here, there is one template parameter, which is a
	``Ptr<const MobilityModel>``.  This tells you that you need a function
	that returns void and takes a ``Ptr<const MobilityModel>``.  For
	example

Υπάρχει μια αντιστοιχία ένα-προς-ένα μεταξύ του καταλόγου παράμετρο προτύπου στη δήλωση και τις επίσημες επιχειρήματα της λειτουργίας επανάκλησης. Εδώ, υπάρχει μια παράμετρο προτύπου, το οποίο είναι ένα `` Ptr <const MobilityModel> ``. Αυτό σας λέει ότι χρειάζεστε μια συνάρτηση που επιστρέφει κενό και παίρνει ένα `` Ptr <const MobilityModel> ``. για παράδειγμα
::

  void
  CourseChange (Ptr<const MobilityModel> model)
  {
    ...
  }

..
	That's all you need if you want to ``Config::ConnectWithoutContext``.
	If you want a context, you need to ``Config::Connect`` and use a
	Callback function that takes a string context, then the template
	arguments

Αυτό είναι το μόνο που χρειάζεστε, αν θέλετε να `` Config :: ConnectWithoutContext``. Αν θέλετε ένα πλαίσιο, θα πρέπει να `` Config :: Connect`` και να χρησιμοποιήσετε μια λειτουργία επανάκλησης που παίρνει ένα πλαίσιο string, τότε τα επιχειρήματα πρότυπο
::

  void
  CourseChange (const std::string context, Ptr<const MobilityModel> model)
  {
    ...
  }

..
	If you want to ensure that your ``CourseChangeCallback`` function is only
	visible in your local file, you can add the keyword ``static`` and
	come up with

Αν θέλετε να εξασφαλίσετε ότι σας `` λειτουργία CourseChangeCallback`` είναι ορατή μόνο σε τοπικό αρχείο σας, μπορείτε να προσθέσετε τη λέξη-κλειδί `` static`` και να καταλήξουμε σε
::

  static void
  CourseChange (const std::string path, Ptr<const MobilityModel> model)
  {
    ...
  }

..
	which is exactly what we used in the ``third.cc`` example.

το οποίο είναι ακριβώς αυτό που χρησιμοποιείται στην `` third.cc`` παράδειγμα.

Implementation
~~~~~~~~~~~~~~

This section is entirely optional.  It is going to be a bumpy ride,
especially for those unfamiliar with the details of templates.
However, if you get through this, you will have a very good handle on
a lot of the |ns3| low level idioms.

So, again, let's figure out what signature of callback function is
required for the "CourseChange" trace source.  This is going to be
painful, but you only need to do this once.  After you get through
this, you will be able to just look at a ``TracedCallback`` and
understand it.

The first thing we need to look at is the declaration of the trace
source.  Recall that this is in ``mobility-model.h``, where we have
previously found

::

  TracedCallback<Ptr<const MobilityModel> > m_courseChangeTrace;

This declaration is for a template.  The template parameter is inside
the angle-brackets, so we are really interested in finding out what
that ``TracedCallback<>`` is.  If you have absolutely no idea where
this might be found, ``grep`` is your friend.

We are probably going to be interested in some kind of declaration in
the |ns3| source, so first change into the ``src`` directory.  Then,
we know this declaration is going to have to be in some kind of header
file, so just ``grep`` for it using:

.. sourcecode:: bash

  $ find . -name '*.h' | xargs grep TracedCallback

You'll see 303 lines fly by (I piped this through ``wc`` to see how bad it
was).  Although that may seem like a lot, that's not really a lot.  Just
pipe the output through ``more`` and start scanning through it.  On the
first page, you will see some very suspiciously template-looking
stuff.

::

  TracedCallback<T1,T2,T3,T4,T5,T6,T7,T8>::TracedCallback ()
  TracedCallback<T1,T2,T3,T4,T5,T6,T7,T8>::ConnectWithoutContext (c ...
  TracedCallback<T1,T2,T3,T4,T5,T6,T7,T8>::Connect (const CallbackB ...
  TracedCallback<T1,T2,T3,T4,T5,T6,T7,T8>::DisconnectWithoutContext ...
  TracedCallback<T1,T2,T3,T4,T5,T6,T7,T8>::Disconnect (const Callba ...
  TracedCallback<T1,T2,T3,T4,T5,T6,T7,T8>::operator() (void) const ...
  TracedCallback<T1,T2,T3,T4,T5,T6,T7,T8>::operator() (T1 a1) const ...
  TracedCallback<T1,T2,T3,T4,T5,T6,T7,T8>::operator() (T1 a1, T2 a2 ...
  TracedCallback<T1,T2,T3,T4,T5,T6,T7,T8>::operator() (T1 a1, T2 a2 ...
  TracedCallback<T1,T2,T3,T4,T5,T6,T7,T8>::operator() (T1 a1, T2 a2 ...
  TracedCallback<T1,T2,T3,T4,T5,T6,T7,T8>::operator() (T1 a1, T2 a2 ...
  TracedCallback<T1,T2,T3,T4,T5,T6,T7,T8>::operator() (T1 a1, T2 a2 ...
  TracedCallback<T1,T2,T3,T4,T5,T6,T7,T8>::operator() (T1 a1, T2 a2 ...

It turns out that all of this comes from the header file
``traced-callback.h`` which sounds very promising.  You can then take
a look at ``mobility-model.h`` and see that there is a line which
confirms this hunch::

  #include "ns3/traced-callback.h"

Of course, you could have gone at this from the other direction and
started by looking at the includes in ``mobility-model.h`` and
noticing the include of ``traced-callback.h`` and inferring that this
must be the file you want.

In either case, the next step is to take a look at
``src/core/model/traced-callback.h`` in your favorite editor to see
what is happening.

You will see a comment at the top of the file that should be
comforting:

  An ns3::TracedCallback has almost exactly the same API as a normal
  ns3::Callback but instead of forwarding calls to a single function
  (as an ns3::Callback normally does), it forwards calls to a chain of
  ns3::Callback.

This should sound very familiar and let you know you are on the right
track.

Just after this comment, you will find

::

  template<typename T1 = empty, typename T2 = empty, 
           typename T3 = empty, typename T4 = empty,
           typename T5 = empty, typename T6 = empty,
           typename T7 = empty, typename T8 = empty>
  class TracedCallback 
  {
    ...

This tells you that TracedCallback is a templated class.  It has eight
possible type parameters with default values.  Go back and compare
this with the declaration you are trying to understand::

  TracedCallback<Ptr<const MobilityModel> > m_courseChangeTrace;

The ``typename T1`` in the templated class declaration corresponds to
the ``Ptr<const MobilityModel>`` in the declaration above.  All of the
other type parameters are left as defaults.  Looking at the
constructor really doesn't tell you much.  The one place where you
have seen a connection made between your Callback function and the
tracing system is in the ``Connect`` and ``ConnectWithoutContext``
functions.  If you scroll down, you will see a
``ConnectWithoutContext`` method here::

  template<typename T1, typename T2, 
           typename T3, typename T4,
           typename T5, typename T6,
           typename T7, typename T8>
  void 
  TracedCallback<T1,T2,T3,T4,T5,T6,T7,T8>::ConnectWithoutContext ...
  {
    Callback<void,T1,T2,T3,T4,T5,T6,T7,T8> cb;
    cb.Assign (callback);
    m_callbackList.push_back (cb);
  }

You are now in the belly of the beast.  When the template is
instantiated for the declaration above, the compiler will replace
``T1`` with ``Ptr<const MobilityModel>``.

::

  void 
  TracedCallback<Ptr<const MobilityModel>::ConnectWithoutContext ... cb
  {
    Callback<void, Ptr<const MobilityModel> > cb;
    cb.Assign (callback);
    m_callbackList.push_back (cb);
  }

You can now see the implementation of everything we've been talking
about.  The code creates a Callback of the right type and assigns your
function to it.  This is the equivalent of the ``pfi = MyFunction`` we
discussed at the start of this section.  The code then adds the
Callback to the list of Callbacks for this source.  The only thing
left is to look at the definition of Callback.  Using the same ``grep``
trick as we used to find ``TracedCallback``, you will be able to find
that the file ``./core/callback.h`` is the one we need to look at.

If you look down through the file, you will see a lot of probably
almost incomprehensible template code.  You will eventually come to
some API Documentation for the Callback template class, though.  Fortunately,
there is some English:

  **Callback** template class.

  This class template implements the Functor Design Pattern. It is used to declare the type of a **Callback**:

  * the first non-optional template argument represents the return type of the callback.
  * the reminaining (optional) template arguments represent the type of the subsequent arguments to the callback.
  * up to nine arguments are supported.

We are trying to figure out what the

::

    Callback<void, Ptr<const MobilityModel> > cb;

declaration means.  Now we are in a position to understand that the
first (non-optional) template argument, ``void``, represents the
return type of the Callback.  The second (optional) template argument,
``Ptr<const MobilityModel>`` represents the type of the first argument
to the callback.

The Callback in question is your function to receive the trace events.
From this you can infer that you need a function that returns ``void``
and takes a ``Ptr<const MobilityModel>``.  For example,

::

  void
  CourseChangeCallback (Ptr<const MobilityModel> model)
  {
    ...
  }

That's all you need if you want to ``Config::ConnectWithoutContext``.
If you want a context, you need to ``Config::Connect`` and use a
Callback function that takes a string context.  This is because the
``Connect`` function will provide the context for you.  You'll need::

  void
  CourseChangeCallback (std::string context, Ptr<const MobilityModel> model)
  {
    ...
  }

If you want to ensure that your ``CourseChangeCallback`` is only
visible in your local file, you can add the keyword ``static`` and
come up with::

  static void
  CourseChangeCallback (std::string path, Ptr<const MobilityModel> model)
  {
    ...
  }

which is exactly what we used in the ``third.cc`` example.  Perhaps
you should now go back and reread the previous section (Take My Word
for It).

If you are interested in more details regarding the implementation of
Callbacks, feel free to take a look at the |ns3| manual.  They are one
of the most frequently used constructs in the low-level parts of
|ns3|.  It is, in my opinion, a quite elegant thing.

TracedValues
++++++++++++

Earlier in this section, we presented a simple piece of code that used
a ``TracedValue<int32_t>`` to demonstrate the basics of the tracing
code.  We just glossed over the what a TracedValue really is and how
to find the return type and formal arguments for the callback.

As we mentioned, the file, ``traced-value.h`` brings in the required
declarations for tracing of data that obeys value semantics.  In
general, value semantics just means that you can pass the object
itself around, rather than passing the address of the object.  We
extend that requirement to include the full set of assignment-style
operators that are pre-defined for plain-old-data (POD) types:

  +---------------------+---------------------+
  | ``operator=`` (assignment)                |
  +---------------------+---------------------+
  | ``operator*=``      | ``operator/=``      |
  +---------------------+---------------------+
  | ``operator+=``      | ``operator-=``      |
  +---------------------+---------------------+
  | ``operator++`` (both prefix and postfix)  |
  +---------------------+---------------------+
  | ``operator--`` (both prefix and postfix)  |
  +---------------------+---------------------+
  | ``operator<<=``     | ``operator>>=``     |
  +---------------------+---------------------+
  | ``operator&=``      | ``operator|=``      |
  +---------------------+---------------------+
  | ``operator%=``      | ``operator^=``      |
  +---------------------+---------------------+

What this all really means is that you will be able to trace all
changes made using those operators to a C++ object which has value
semantics.

The ``TracedValue<>`` declaration we saw above provides the
infrastructure that overloads the operators mentioned above and drives
the callback process.  On use of any of the operators above with a
``TracedValue`` it will provide both the old and the new value of that
variable, in this case an ``int32_t`` value.  By inspection of the
``TracedValue`` declaration, we know the trace sink function will have
arguments ``(const int32_t oldValue, const int32_t newValue)``.  The
return type for a ``TracedValue`` callback function is always
``void``, so the expected callback signature will be::

  void (* TracedValueCallback)(const int32_t oldValue, const int32_t newValue);

The ``.AddTraceSource`` in the ``GetTypeId`` method provides the
"hooks" used for connecting the trace source to the outside world
through the Config system.  We already discussed the first three
agruments to ``AddTraceSource``: the Attribute name for the Config
system, a help string, and the address of the TracedValue class data
member.

The final string argument, "ns3::Traced::Value::Int32" in the example,
is the name of a ``typedef`` for the callback function signature.  We
require these signatures to be defined, and give the fully qualified
type name to ``AddTraceSource``, so the API documentation can link a
trace source to the function signature.  For TracedValue the signature
is straightforward; for TracedCallbacks we've already seen the API
docs really help.

Real Example
************

Let's do an example taken from one of the best-known books on TCP
around.  "TCP/IP Illustrated, Volume 1: The Protocols," by W. Richard
Stevens is a classic.  I just flipped the book open and ran across a
nice plot of both the congestion window and sequence numbers versus
time on page 366.  Stevens calls this, "Figure 21.10. Value of cwnd
and send sequence number while data is being transmitted."  Let's just
recreate the cwnd part of that plot in |ns3| using the tracing system
and ``gnuplot``.

Available Sources
+++++++++++++++++

The first thing to think about is how we want to get the data out.
What is it that we need to trace?  So let's consult "All Trace
Sources" list to see what we have to work with.  Recall that this is
found in the |ns3| API Documentation.  If you scroll through the list,
you will eventually find:

  **ns3::TcpNewReno**

  * **CongestionWindow**: The TCP connection's congestion window
  * **SlowStartThreshold**: TCP slow start threshold (bytes)

It turns out that the |ns3| TCP implementation lives (mostly) in the
file ``src/internet/model/tcp-socket-base.cc`` while congestion
control variants are in files such as
``src/internet/model/tcp-newreno.cc``.  If you don't know this *a
priori*, you can use the recursive ``grep`` trick:

.. sourcecode:: bash

  $ find . -name '*.cc' | xargs grep -i tcp

You will find page after page of instances of tcp pointing you to that
file.

Bringing up the class documentation for ``TcpNewReno`` and skipping to
the list of TraceSources you will find

  **TraceSources**

  * **CongestionWindow**: The TCP connnection's congestion window

    Callback signature:  **ns3::Traced::Value::Uint322Callback**

Clicking on the callback ``typedef`` link we see the signature
you now know to expect::

    typedef void(* ns3::Traced::Value::Int32Callback)(const int32_t oldValue, const int32_t newValue)

You should now understand this code completely.  If we have a pointer
to the ``TcpNewReno``, we can ``TraceConnect`` to the
"CongestionWindow" trace source if we provide an appropriate callback
target.  This is the same kind of trace source that we saw in the
simple example at the start of this section, except that we are
talking about ``uint32_t`` instead of ``int32_t``.  And we know
that we have to provide a callback function with that signature.

Finding Examples
++++++++++++++++

It's always best to try and find working code laying around that you
can modify, rather than starting from scratch.  So the first order of
business now is to find some code that already hooks the
"CongestionWindow" trace source and see if we can modify it.  As
usual, ``grep`` is your friend:

.. sourcecode:: bash

  $ find . -name '*.cc' | xargs grep CongestionWindow

This will point out a couple of promising candidates: 
``examples/tcp/tcp-large-transfer.cc`` and 
``src/test/ns3tcp/ns3tcp-cwnd-test-suite.cc``.

We haven't visited any of the test code yet, so let's take a look
there.  You will typically find that test code is fairly minimal, so
this is probably a very good bet.  Open
``src/test/ns3tcp/ns3tcp-cwnd-test-suite.cc`` in your favorite editor
and search for "CongestionWindow".  You will find,

::

  ns3TcpSocket->TraceConnectWithoutContext ("CongestionWindow", 
    MakeCallback (&Ns3TcpCwndTestCase1::CwndChange, this));

This should look very familiar to you.  We mentioned above that if we
had a pointer to the ``TcpNewReno``, we could ``TraceConnect`` to the
"CongestionWindow" trace source.  That's exactly what we have here; so
it turns out that this line of code does exactly what we want.  Let's
go ahead and extract the code we need from this function
(``Ns3TcpCwndTestCase1::DoRun (void)``).  If you look at this
function, you will find that it looks just like an |ns3| script.  It
turns out that is exactly what it is.  It is a script run by the test
framework, so we can just pull it out and wrap it in ``main`` instead
of in ``DoRun``.  Rather than walk through this, step, by step, we
have provided the file that results from porting this test back to a
native |ns3| script -- ``examples/tutorial/fifth.cc``.

Dynamic Trace Sources
+++++++++++++++++++++

The ``fifth.cc`` example demonstrates an extremely important rule that
you must understand before using any kind of trace source: you must
ensure that the target of a ``Config::Connect`` command exists before
trying to use it.  This is no different than saying an object must be
instantiated before trying to call it.  Although this may seem obvious
when stated this way, it does trip up many people trying to use the
system for the first time.

Let's return to basics for a moment.  There are three basic execution
phases that exist in any |ns3| script.  The first phase is
sometimes called "Configuration Time" or "Setup Time," and exists
during the period when the ``main`` function of your script is
running, but before ``Simulator::Run`` is called.  The second phase
is sometimes called "Simulation Time" and exists during
the time period when ``Simulator::Run`` is actively executing its
events.  After it completes executing the simulation,
``Simulator::Run`` will return control back to the ``main`` function.
When this happens, the script enters what can be called the "Teardown
Phase," which is when the structures and objects created during setup
are taken apart and released.

Perhaps the most common mistake made in trying to use the tracing
system is assuming that entities constructed dynamically *during
simulation time* are available during configuration time.  In
particular, an |ns3| ``Socket`` is a dynamic object often created by
``Applications`` to communicate between ``Nodes``.  An |ns3|
``Application`` always has a "Start Time" and a "Stop Time" associated
with it.  In the vast majority of cases, an ``Application`` will not
attempt to create a dynamic object until its ``StartApplication``
method is called at some "Start Time".  This is to ensure that the
simulation is completely configured before the app tries to do
anything (what would happen if it tried to connect to a Node that
didn't exist yet during configuration time?).  As a result, during the
configuration phase you can't connect a trace source to a trace sink
if one of them is created dynamically during the simulation.

The two solutions to this connundrum are

#. Create a simulator event that is run after the dynamic object is
   created and hook the trace when that event is executed; or
#. Create the dynamic object at configuration time, hook it then, and
   give the object to the system to use during simulation time.

We took the second approach in the ``fifth.cc`` example.  This
decision required us to create the ``MyApp`` ``Application``, the
entire purpose of which is to take a ``Socket`` as a parameter.

Walkthrough: ``fifth.cc``
+++++++++++++++++++++++++

Now, let's take a look at the example program we constructed by
dissecting the congestion window test.  Open
``examples/tutorial/fifth.cc`` in your favorite editor.  You should
see some familiar looking code::

  /* -*- Mode:C++; c-file-style:"gnu"; indent-tabs-mode:nil; -*- */
  /*
   * This program is free software; you can redistribute it and/or modify
   * it under the terms of the GNU General Public License version 2 as
   * published by the Free Software Foundation;
   *
   * This program is distributed in the hope that it will be useful,
   * but WITHOUT ANY WARRANTY; without even the implied warranty of
   * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
   * GNU General Public License for more details.
   *
   * You should have received a copy of the GNU General Public License
   * along with this program; if not, write to the Free Software
   * Foundation, Include., 59 Temple Place, Suite 330, Boston, MA  02111-1307  USA
   */
  
  #include <fstream>
  #include "ns3/core-module.h"
  #include "ns3/network-module.h"
  #include "ns3/internet-module.h"
  #include "ns3/point-to-point-module.h"
  #include "ns3/applications-module.h"
  
  using namespace ns3;
  
  NS_LOG_COMPONENT_DEFINE ("FifthScriptExample");

This has all been covered, so we won't rehash it.  The next lines of
source are the network illustration and a comment addressing the
problem described above with ``Socket``.

::

  // ===========================================================================
  //
  //         node 0                 node 1
  //   +----------------+    +----------------+
  //   |    ns-3 TCP    |    |    ns-3 TCP    |
  //   +----------------+    +----------------+
  //   |    10.1.1.1    |    |    10.1.1.2    |
  //   +----------------+    +----------------+
  //   | point-to-point |    | point-to-point |
  //   +----------------+    +----------------+
  //           |                     |
  //           +---------------------+
  //                5 Mbps, 2 ms
  //
  //
  // We want to look at changes in the ns-3 TCP congestion window.  We need
  // to crank up a flow and hook the CongestionWindow attribute on the socket
  // of the sender.  Normally one would use an on-off application to generate a
  // flow, but this has a couple of problems.  First, the socket of the on-off
  // application is not created until Application Start time, so we wouldn't be
  // able to hook the socket (now) at configuration time.  Second, even if we
  // could arrange a call after start time, the socket is not public so we
  // couldn't get at it.
  //
  // So, we can cook up a simple version of the on-off application that does what
  // we want.  On the plus side we don't need all of the complexity of the on-off
  // application.  On the minus side, we don't have a helper, so we have to get
  // a little more involved in the details, but this is trivial.
  //
  // So first, we create a socket and do the trace connect on it; then we pass
  // this socket into the constructor of our simple application which we then
  // install in the source node.
  // ===========================================================================
  //

This should also be self-explanatory.  

The next part is the declaration of the ``MyApp`` ``Application`` that
we put together to allow the ``Socket`` to be created at configuration
time.

::

  class MyApp : public Application
  {
  public:
  
    MyApp ();
    virtual ~MyApp();
  
    void Setup (Ptr<Socket> socket, Address address, uint32_t packetSize, 
      uint32_t nPackets, DataRate dataRate);
  
  private:
    virtual void StartApplication (void);
    virtual void StopApplication (void);
  
    void ScheduleTx (void);
    void SendPacket (void);
  
    Ptr<Socket>     m_socket;
    Address         m_peer;
    uint32_t        m_packetSize;
    uint32_t        m_nPackets;
    DataRate        m_dataRate;
    EventId         m_sendEvent;
    bool            m_running;
    uint32_t        m_packetsSent;
  };

You can see that this class inherits from the |ns3| ``Application``
class.  Take a look at ``src/network/model/application.h`` if you are
interested in what is inherited.  The ``MyApp`` class is obligated to
override the ``StartApplication`` and ``StopApplication`` methods.
These methods are automatically called when ``MyApp`` is required to
start and stop sending data during the simulation.

Starting/Stopping Applications
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It is worthwhile to spend a bit of time explaining how events actually
get started in the system.  This is another fairly deep explanation,
and can be ignored if you aren't planning on venturing down into the
guts of the system.  It is useful, however, in that the discussion
touches on how some very important parts of |ns3| work and exposes
some important idioms.  If you are planning on implementing new
models, you probably want to understand this section.

The most common way to start pumping events is to start an
``Application``.  This is done as the result of the following
(hopefully) familar lines of an |ns3| script::

  ApplicationContainer apps = ...
  apps.Start (Seconds (1.0));
  apps.Stop (Seconds (10.0));

The application container code (see
``src/network/helper/application-container.h`` if you are interested)
loops through its contained applications and calls,

::

  app->SetStartTime (startTime);

as a result of the ``apps.Start`` call and

::

  app->SetStopTime (stopTime);

as a result of the ``apps.Stop`` call.

The ultimate result of these calls is that we want to have the
simulator automatically make calls into our ``Applications`` to tell
them when to start and stop.  In the case of ``MyApp``, it inherits
from class ``Application`` and overrides ``StartApplication``, and
``StopApplication``.  These are the functions that will be called by
the simulator at the appropriate time.  In the case of ``MyApp`` you
will find that ``MyApp::StartApplication`` does the initial ``Bind``,
and ``Connect`` on the socket, and then starts data flowing by calling
``MyApp::SendPacket``.  ``MyApp::StopApplication`` stops generating
packets by cancelling any pending send events then closes the socket.

One of the nice things about |ns3| is that you can completely ignore
the implementation details of how your ``Application`` is
"automagically" called by the simulator at the correct time.  But
since we have already ventured deep into |ns3| already, let's go for
it.

If you look at ``src/network/model/application.cc`` you will find that
the ``SetStartTime`` method of an ``Application`` just sets the member
variable ``m_startTime`` and the ``SetStopTime`` method just sets
``m_stopTime``.  From there, without some hints, the trail will
probably end.

The key to picking up the trail again is to know that there is a
global list of all of the nodes in the system.  Whenever you create a
node in a simulation, a pointer to that Node is added to the global
``NodeList``.

Take a look at ``src/network/model/node-list.cc`` and search for
``NodeList::Add``.  The public static implementation calls into a
private implementation called ``NodeListPriv::Add``.  This is a
relatively common idom in |ns3|.  So, take a look at
``NodeListPriv::Add``.  There you will find,

::

  Simulator::ScheduleWithContext (index, TimeStep (0), &Node::Initialize, node);

This tells you that whenever a Node is created in a simulation, as
a side-effect, a call to that node's ``Initialize`` method is
scheduled for you that happens at time zero.  Don't read too much into
that name, yet.  It doesn't mean that the Node is going to start doing
anything, it can be interpreted as an informational call into the
Node telling it that the simulation has started, not a call for
action telling the Node to start doing something.

So, ``NodeList::Add`` indirectly schedules a call to
``Node::Initialize`` at time zero to advise a new Node that the
simulation has started.  If you look in ``src/network/model/node.h``
you will, however, not find a method called ``Node::Initialize``.  It
turns out that the ``Initialize`` method is inherited from class
``Object``.  All objects in the system can be notified when the
simulation starts, and objects of class Node are just one kind of
those objects.

Take a look at ``src/core/model/object.cc`` next and search for
``Object::Initialize``.  This code is not as straightforward as you
might have expected since |ns3| ``Objects`` support aggregation.  The
code in ``Object::Initialize`` then loops through all of the objects
that have been aggregated together and calls their ``DoInitialize``
method.  This is another idiom that is very common in |ns3|, sometimes
called the "template design pattern.": a public non-virtual API
method, which stays constant across implementations, and that calls a
private virtual implementation method that is inherited and
implemented by subclasses.  The names are typically something like
``MethodName`` for the public API and ``DoMethodName`` for the private
API.

This tells us that we should look for a ``Node::DoInitialize`` method
in ``src/network/model/node.cc`` for the method that will continue our
trail.  If you locate the code, you will find a method that loops
through all of the devices in the Node and then all of the
applications in the Node calling ``device->Initialize`` and
``application->Initialize`` respectively.

You may already know that classes ``Device`` and ``Application`` both
inherit from class ``Object`` and so the next step will be to look at
what happens when ``Application::DoInitialize`` is called.  Take a
look at ``src/network/model/application.cc`` and you will find::

  void
  Application::DoInitialize (void)
  {
    m_startEvent = Simulator::Schedule (m_startTime, &Application::StartApplication, this);
    if (m_stopTime != TimeStep (0))
      {
        m_stopEvent = Simulator::Schedule (m_stopTime, &Application::StopApplication, this);
      }
    Object::DoInitialize ();
  }

Here, we finally come to the end of the trail.  If you have kept it
all straight, when you implement an |ns3| ``Application``, your new
application inherits from class ``Application``.  You override the
``StartApplication`` and ``StopApplication`` methods and provide
mechanisms for starting and stopping the flow of data out of your new
``Application``.  When a Node is created in the simulation, it is
added to a global ``NodeList``.  The act of adding a Node to this
``NodeList`` causes a simulator event to be scheduled for time zero
which calls the ``Node::Initialize`` method of the newly added
Node to be called when the simulation starts.  Since a Node
inherits from ``Object``, this calls the ``Object::Initialize`` method
on the Node which, in turn, calls the ``DoInitialize`` methods on
all of the ``Objects`` aggregated to the Node (think mobility
models).  Since the Node ``Object`` has overridden
``DoInitialize``, that method is called when the simulation starts.
The ``Node::DoInitialize`` method calls the ``Initialize`` methods of
all of the ``Applications`` on the node.  Since ``Applications`` are
also ``Objects``, this causes ``Application::DoInitialize`` to be
called.  When ``Application::DoInitialize`` is called, it schedules
events for the ``StartApplication`` and ``StopApplication`` calls on
the ``Application``.  These calls are designed to start and stop the
flow of data from the ``Application``

This has been another fairly long journey, but it only has to be made
once, and you now understand another very deep piece of |ns3|.

The MyApp Application
~~~~~~~~~~~~~~~~~~~~~

The ``MyApp`` ``Application`` needs a constructor and a destructor, of
course::

  MyApp::MyApp ()
    : m_socket (0),
      m_peer (),
      m_packetSize (0),
      m_nPackets (0),
      m_dataRate (0),
      m_sendEvent (),
      m_running (false),
      m_packetsSent (0)
  {
  }
  
  MyApp::~MyApp()
  {
    m_socket = 0;
  }

The existence of the next bit of code is the whole reason why we wrote
this ``Application`` in the first place.

::

  void
  MyApp::Setup (Ptr<Socket> socket, Address address, uint32_t packetSize, 
                       uint32_t nPackets, DataRate dataRate)
  {
    m_socket = socket;
    m_peer = address;
    m_packetSize = packetSize;
    m_nPackets = nPackets;
    m_dataRate = dataRate;
  }
  
This code should be pretty self-explanatory.  We are just initializing
member variables.  The important one from the perspective of tracing
is the ``Ptr<Socket> socket`` which we needed to provide to the
application during configuration time.  Recall that we are going to
create the ``Socket`` as a ``TcpSocket`` (which is implemented by
``TcpNewReno``) and hook its "CongestionWindow" trace source before
passing it to the ``Setup`` method.

::

  void
  MyApp::StartApplication (void)
  {
    m_running = true;
    m_packetsSent = 0;
    m_socket->Bind ();
    m_socket->Connect (m_peer);
    SendPacket ();
  }

The above code is the overridden implementation
``Application::StartApplication`` that will be automatically called by
the simulator to start our ``Application`` running at the appropriate
time.  You can see that it does a ``Socket`` ``Bind`` operation.  If
you are familiar with Berkeley Sockets this shouldn't be a surprise.
It performs the required work on the local side of the connection just
as you might expect.  The following ``Connect`` will do what is
required to establish a connection with the TCP at ``Address`` m_peer.
It should now be clear why we need to defer a lot of this to
simulation time, since the ``Connect`` is going to need a fully
functioning network to complete.  After the ``Connect``, the
``Application`` then starts creating simulation events by calling
``SendPacket``.

The next bit of code explains to the ``Application`` how to stop
creating simulation events.

::

  void
  MyApp::StopApplication (void)
  {
    m_running = false;
  
    if (m_sendEvent.IsRunning ())
      {
        Simulator::Cancel (m_sendEvent);
      }
  
    if (m_socket)
      {
        m_socket->Close ();
      }
  }

Every time a simulation event is scheduled, an ``Event`` is created.
If the ``Event`` is pending execution or executing, its method
``IsRunning`` will return ``true``.  In this code, if ``IsRunning()``
returns true, we ``Cancel`` the event which removes it from the
simulator event queue.  By doing this, we break the chain of events
that the ``Application`` is using to keep sending its ``Packets`` and
the ``Application`` goes quiet.  After we quiet the ``Application`` we
``Close`` the socket which tears down the TCP connection.

The socket is actually deleted in the destructor when the ``m_socket =
0`` is executed.  This removes the last reference to the underlying
Ptr<Socket> which causes the destructor of that Object to be called.

Recall that ``StartApplication`` called ``SendPacket`` to start the
chain of events that describes the ``Application`` behavior.

::

  void
  MyApp::SendPacket (void)
  {
    Ptr<Packet> packet = Create<Packet> (m_packetSize);
    m_socket->Send (packet);
  
    if (++m_packetsSent < m_nPackets)
      {
        ScheduleTx ();
      }
  }

Here, you see that ``SendPacket`` does just that.  It creates a
``Packet`` and then does a ``Send`` which, if you know Berkeley
Sockets, is probably just what you expected to see.

It is the responsibility of the ``Application`` to keep scheduling the
chain of events, so the next lines call ``ScheduleTx`` to schedule
another transmit event (a ``SendPacket``) until the ``Application``
decides it has sent enough.

::

  void
  MyApp::ScheduleTx (void)
  {
    if (m_running)
      {
        Time tNext (Seconds (m_packetSize * 8 / static_cast<double> (m_dataRate.GetBitRate ())));
        m_sendEvent = Simulator::Schedule (tNext, &MyApp::SendPacket, this);
      }
  }

Here, you see that ``ScheduleTx`` does exactly that.  If the
``Application`` is running (if ``StopApplication`` has not been
called) it will schedule a new event, which calls ``SendPacket``
again.  The alert reader will spot something that also trips up new
users.  The data rate of an ``Application`` is just that.  It has
nothing to do with the data rate of an underlying ``Channel``.  This
is the rate at which the ``Application`` produces bits.  It does not
take into account any overhead for the various protocols or channels
that it uses to transport the data.  If you set the data rate of an
``Application`` to the same data rate as your underlying ``Channel``
you will eventually get a buffer overflow.
