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
  > Last update was performed at 30-04-2015 by [Vasileios Dimitropoulos].
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
++++++++++++++++++++++

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
δεν είναι παρών σε υφιστάμενες εξόδους αρχείων καταγραφής, μπορείτε να επεξεργαστείτε τον πυρήνα 
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
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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
ορίσματα της συνάρτησης επανάκλησης μου;*

..
  The easiest way is to examine the callback signature ``typedef``,
  which is given in the "Callback signature" of the trace source in the
  "Detailed Description" for the class, as shown above.

Ο ευκολότερος τρόπος είναι να εξετάσετε την υπογραφή επανάκλησης ``typedef``, η οποία δίνεται στη "Callback signature" της 
πηγής ίχνους στο "Detailed Description" της κλάσης, όπως φαίνεται παραπάνω.

..
  Repeating the "CourseChange" trace source entry from
  ``ns3::RandomWalk2dMobilityModel`` we have:
  
Η επανάληψη καταχώρησης πηγής ίχνους της "CourseChange" από ``ns3::RandomWalk2dMobilityModel`` έχουμε:

  * **CourseChange**: The value of the position and/or velocity vector
    changed.

    Callback signature: ``ns3::MobilityModel::CourseChangeCallback``

..
  The callback signature is given as a link to the relevant ``typedef``,
  where we find

Η υπογραφή ή το στίγμα επανάκλησης δίνεται ως ένας σύνδεσμος με τη σχετική ``typedef``, όπου βρίσκουμε

  ``typedef void (* CourseChangeCallback)(const std::string context, Ptr<const MobilityModel> * model);``
            
  **TracedCallback** signature for course change notifications.

  ..
    If the callback is connected using ``ConnectWithoutContext`` omit the 
    ``context`` argument from the signature.
  
  Αν η επανάκληση συνδέεται με τη χρήση ``ConnectWithoutContext`` παραλείψτε το ``context`` όρισμα από το στίγμα.

  **Parameters**:

      |  [in] :param:`context` The context string supplied by the Trace source.
      |  [in] :param:`model` The MobilityModel which is changing course.

..
  As above, to see this in use ``grep`` around in the |ns3| codebase for
  an example.  The example above, from
  ``src/mobility/examples/main-random-topology.cc``, connects the
  "CourseChange" trace source to the ``CourseChange`` function in the
  same file

Όπως και παραπάνω, για να δείτε αυτό κατά τη χρήση ``grep`` γύρω στον κώδικα βάσης |ns3| για παράδειγμα. Το παραπάνω 
παράδειγμα, από ``src/mobility/examples/main-random-topology.cc``, συνδέει την πηγή ίχνους "CourseChange" στην συνάρτηση 
``CourseChange`` στο ίδιο αρχείο
::

  static void
  CourseChange (std::string context, Ptr<const MobilityModel> model)
  {
    ...
  }

..
  Notice that this function:

Σημειώστε ότι αυτή η συνάρτηση:

..
  * Takes a "context" string argument, which we'll describe in a minute.
    (If the callback is connected using the ``ConnectWithoutContext``
    function the ``context`` argument will be omitted.)
  
  * Παίρνει ένα όρισμα (string) "περιεχόμενο", το οποίο θα περιγράψουμε σε ένα λεπτό. 
  (Αν η επανάκληση συνδέεται με τη χρήση της συνάρτησης ``ConnectWithoutContext`` το όρισμα ``context`` θα πρέπει 
  να παραλείπεται.)

..
  * Has the ``MobilityModel`` supplied as the last argument (or only
    argument if ``ConnectWithoutContext`` is used).
  
  * Έχει την ``MobilityModel`` η οποία παρέχεται ως το τελευταίο όρισμα (ή μόνο το όρισμα εάν χρησιμοποιείται το ``ConnectWithoutContext``).

..
  * Returns ``void``.

* Επιστρέφει ``void``.

..
  If, by chance, the callback signature hasn't been documented, and
  there are no examples to work from, determining the right callback
  function signature can be, well, challenging to actually figure out
  from the source code.

Αν, κατά τύχη, το στίγμα επανάκλησης δεν έχει τεκμηριωθεί, και δεν υπάρχουν παραδείγματα για εργασία, καθόρισε 
το σωστό στίγμα της συνάρτησης επανάκλησης που μπορεί να είναι, επίσης, δύσκολο πραγματικά να υπολογιστεί 
από τον πηγαίο κώδικα.

..
  Before embarking on a walkthrough of the code, I'll be kind and just
  tell you a simple way to figure this out: The return value of your
  callback will always be ``void``.  The formal parameter list for a
  ``TracedCallback`` can be found from the template parameter list in
  the declaration.  Recall that for our current example, this is in
  ``mobility-model.h``, where we have previously found

Πριν προβώντας σε μια περιδιάβαση του κώδικα, θα είμαι ευθύς και απλά να σας πω έναν απλό τρόπο για να το καταλάβετε: Η 
τιμή επιστροφής της επανάκλησής σας θα είναι πάντα ``void``. Η επίσημη λίστα παραμέτρων για μια ``TracedCallback`` μπορεί 
να βρεθεί από τη λίστα της παραμέτρου προτύπου στη δήλωση. Υπενθυμίζεται ότι για το τρέχον παράδειγμά μας, αυτό είναι 
``mobility-model.h``, όπου βρήκαμε προηγουμένως
::

  TracedCallback<Ptr<const MobilityModel> > m_courseChangeTrace;

..
  There is a one-to-one correspondence between the template parameter
  list in the declaration and the formal arguments of the callback
  function.  Here, there is one template parameter, which is a
  ``Ptr<const MobilityModel>``.  This tells you that you need a function
  that returns void and takes a ``Ptr<const MobilityModel>``.  For
  example

Υπάρχει μια αντιστοιχία ένα-προς-ένα μεταξύ της λίστας προτύπου παραμέτρου στη δήλωση και τα επίσημα ορίσματα της 
συνάρτησης επανάκλησης. Εδώ, υπάρχει μια παράμετρος προτύπου, η οποία είναι ένα ``Ptr<const MobilityModel>``. Αυτό σας λέει 
ότι χρειάζεστε μια συνάρτηση που επιστρέφει κενό και παίρνει ένα ``Ptr<const MobilityModel>``. Για παράδειγμα
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

Αυτό είναι το μόνο που χρειάζεστε, αν θέλετε να ``Config::ConnectWithoutContext``. Αν θέλετε ένα περιεχόμενο, θα πρέπει να 
``Config::Connect`` και να χρησιμοποιήσετε μια συνάρτηση Επανάκλησης που παίρνει ένα περιεχόμενο string, τότε τα ορίσματα 
πρότυπα
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

Αν θέλετε να εξασφαλίσετε ότι η συνάρτησή σας ``CourseChangeCallback`` είναι ορατή μόνο σε τοπικό αρχείο σας, μπορείτε να 
προσθέσετε τη λέξη-κλειδί ``static`` και να καταλήξετε σε
::

  static void
  CourseChange (const std::string path, Ptr<const MobilityModel> model)
  {
    ...
  }

..
  which is exactly what we used in the ``third.cc`` example.

το οποίο είναι ακριβώς αυτό που χρησιμοποιείται στο παράδειγμα ``third.cc``.

..
  Implementation

Εφαρμογή
~~~~~~~~

..
  This section is entirely optional.  It is going to be a bumpy ride,
  especially for those unfamiliar with the details of templates.
  However, if you get through this, you will have a very good handle on
  a lot of the |ns3| low level idioms.

Αυτή η ενότητα είναι εντελώς προαιρετική. Πρόκειται να είναι μια δύσκολη διαδρομή, ειδικά για όσους δεν είναι εξοικειωμένοι 
με τις λεπτομέρειες των προτύπων. Ωστόσο, εάν μπορείτε να πάρετε μέσα από αυτό, θα έχετε μια πολύ καλή λαβή για πολλά από τα 
|ns3| ιδιώματα χαμηλού επιπέδου.

..
  So, again, let's figure out what signature of callback function is
  required for the "CourseChange" trace source.  This is going to be
  painful, but you only need to do this once.  After you get through
  this, you will be able to just look at a ``TracedCallback`` and
  understand it.

Έτσι, και πάλι, ας υπολογίσουμε τι στίγμα της συνάρτησης επανάκλησης απαιτείται για την πηγή ίχνους "CourseChange". Αυτό 
πρόκειται να είναι οδυνηρό, αλλά χρειάζεται να το κάνετε αυτό μια φορά. Μετά μπορείτε να πάρετε μέσα από αυτό, θα είστε σε 
θέση να εξετάσετε ένα ``TracedCallback`` και να το κατανοήσετε.

..
  The first thing we need to look at is the declaration of the trace
  source.  Recall that this is in ``mobility-model.h``, where we have
  previously found

Το πρώτο πράγμα που πρέπει να εξετάσουμε είναι η δήλωση της πηγής ίχνους. Θυμηθείτε ότι αυτό είναι ``mobility-model.h``, 
όπου έχουμε διαπιστώσει στο παρελθόν
::

  TracedCallback<Ptr<const MobilityModel> > m_courseChangeTrace;

..
  This declaration is for a template.  The template parameter is inside
  the angle-brackets, so we are really interested in finding out what
  that ``TracedCallback<>`` is.  If you have absolutely no idea where
  this might be found, ``grep`` is your friend.

Αυτή η δήλωση είναι για ένα πρότυπο. Η παράμετρος πρότυπο είναι μέσα στη παρένθεση, ώστε πραγματικά ενδιαφερόμαστε να 
ανακαλύψουμε τι είναι το ``TracedCallback<>``. Αν δεν έχετε απολύτως καμία ιδέα για το πού αυτό θα μπορούσε να βρεθεί, 
το ``grep`` είναι ο φίλος σου.

..
  We are probably going to be interested in some kind of declaration in
  the |ns3| source, so first change into the ``src`` directory.  Then,
  we know this declaration is going to have to be in some kind of header
  file, so just ``grep`` for it using:

Πρόκειται πιθανώς να ασχοληθούμε για κάποιο είδος δήλωσης της πηγής |ns3|, οπότε πρώτη αλλαγή στον κατάλογο ``src``. Στη 
συνέχεια, γνωρίζουμε ότι η δήλωση αυτή θα πρέπει να είναι σε κάποιο είδος του αρχείου header, έτσι απλά ``grep`` για να το 
χρησιμοποιείτε:

.. sourcecode:: bash

  $ find . -name '*.h' | xargs grep TracedCallback

..
  You'll see 303 lines fly by (I piped this through ``wc`` to see how bad it
  was).  Although that may seem like a lot, that's not really a lot.  Just
  pipe the output through ``more`` and start scanning through it.  On the
  first page, you will see some very suspiciously template-looking
  stuff.

Θα δείτε 303 γραμμές να πετούν από (σας δείχνω αυτό μέσω ``wc`` για να δείτε πόσο άσχημο ήταν). Αν και αυτά μπορεί να 
φαίνονται πολλά, αλλά αυτά δεν είναι πραγματικά πολλά. Απλά διοχέτευσε την έξοδο μέσω ``more`` και ξεκινήστε τη σάρωση μέσα 
από αυτό. Στην πρώτη σελίδα, θα δείτε κάποια πολύ ύποπτα πρότυπα - να αναζητούν πράγματα.
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

..
  It turns out that all of this comes from the header file
  ``traced-callback.h`` which sounds very promising.  You can then take
  a look at ``mobility-model.h`` and see that there is a line which
  confirms this hunch

Αποδεικνύεται ότι όλο αυτό προέρχεται από το αρχείο κεφαλίδας ``traced-callback.h`` που ακούγεται πολύ ελπιδοφόρα. Στη 
συνέχεια μπορείτε να ρίξετε μια ματιά στο ``mobility-model.h`` και να δούμε ότι υπάρχει μια γραμμή που επιβεβαιώνει αυτό το 
προαίσθημα
::

  #include "ns3/traced-callback.h"

..
  Of course, you could have gone at this from the other direction and
  started by looking at the includes in ``mobility-model.h`` and
  noticing the include of ``traced-callback.h`` and inferring that this
  must be the file you want.

Φυσικά, θα μπορούσατε να έχετε πάει σε αυτό από την άλλη κατεύθυνση και να αρχίσετε κοιτάζοντας τα περιεχόμενα του 
``mobility-model.h`` και παρατηρώντας το περιεχόμενο του ``traced-callback.h`` και να συμπεράνετε ότι αυτό πρέπει να είναι 
το αρχείο που θέλετε.

..
  In either case, the next step is to take a look at
  ``src/core/model/traced-callback.h`` in your favorite editor to see
  what is happening.

Σε κάθε περίπτωση, το επόμενο βήμα είναι να ρίξετε μια ματιά στο ``src/core/model/traced-callback.h`` στο αγαπημένο σας 
επεξεργαστή κειμένου για να δείτε τι συμβαίνει.

..
  You will see a comment at the top of the file that should be
  comforting:

Θα δείτε ένα σχόλιο στην αρχή του αρχείου που θα πρέπει να είναι παρήγορο:

    An ns3::TracedCallback has almost exactly the same API as a normal
    ns3::Callback but instead of forwarding calls to a single function
    (as an ns3::Callback normally does), it forwards calls to a chain of
    ns3::Callback.
  
..
  This should sound very familiar and let you know you are on the right
  track.

Αυτό θα πρέπει να ακούγεται πολύ οικείο και να ξέρετε ότι είστε στο σωστό δρόμο.

..
  Just after this comment, you will find

Λίγο μετά από αυτό το σχόλιο, θα βρείτε
::

  template<typename T1 = empty, typename T2 = empty, 
           typename T3 = empty, typename T4 = empty,
           typename T5 = empty, typename T6 = empty,
           typename T7 = empty, typename T8 = empty>
  class TracedCallback 
  {
    ...

..
  This tells you that TracedCallback is a templated class.  It has eight
  possible type parameters with default values.  Go back and compare
  this with the declaration you are trying to understand

Αυτό σας λέει ότι TracedCallback είναι templated κλάση. Έχει οκτώ πιθανές τύπου παραμέτρους με προκαθορισμένες τιμές. 
Πηγαίνετε πίσω και να την συγκρίνετε με τη δήλωση που προσπαθείτε να καταλάβετε
::

  TracedCallback<Ptr<const MobilityModel> > m_courseChangeTrace;

..
  The ``typename T1`` in the templated class declaration corresponds to
  the ``Ptr<const MobilityModel>`` in the declaration above.  All of the
  other type parameters are left as defaults.  Looking at the
  constructor really doesn't tell you much.  The one place where you
  have seen a connection made between your Callback function and the
  tracing system is in the ``Connect`` and ``ConnectWithoutContext``
  functions.  If you scroll down, you will see a
  ``ConnectWithoutContext`` method here

Το ``typename T1`` στην templated δήλωση της κλάσης αντιστοιχεί στο ``Ptr<const MobilityModel>`` στην παραπάνω δήλωση. 
Όλες οι άλλες παράμετροι τύπου παραμένουν ως προεπιλογές. Κοιτάζοντας τον κατασκευαστή πραγματικά δεν σας λέει πολλά. Το 
ένα μέρος όπου μπορείτε να έχετε δει μια σύνδεση μεταξύ της συνάρτησης επανάκλησής σας και το σύστημα εντοπισμού είναι σε 
``Connect`` και οι συναρτήσεις ``ConnectWithoutContext``. Αν μετακινηθείτε προς τα κάτω, θα δείτε μια μέθοδο ``ConnectWithoutContext`` εδώ
::

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

..
  You are now in the belly of the beast.  When the template is
  instantiated for the declaration above, the compiler will replace
  ``T1`` with ``Ptr<const MobilityModel>``.

Είστε τώρα στην κοιλιά του κτήνους. Όταν το πρότυπο έχει παραδείγματα για τη δήλωση παραπάνω, ο compiler θα αντικαταστήσει 
τον ``T1`` με ``Ptr<const MobilityModel>``.
::

  void 
  TracedCallback<Ptr<const MobilityModel>::ConnectWithoutContext ... cb
  {
    Callback<void, Ptr<const MobilityModel> > cb;
    cb.Assign (callback);
    m_callbackList.push_back (cb);
  }

..
  You can now see the implementation of everything we've been talking
  about.  The code creates a Callback of the right type and assigns your
  function to it.  This is the equivalent of the ``pfi = MyFunction`` we
  discussed at the start of this section.  The code then adds the
  Callback to the list of Callbacks for this source.  The only thing
  left is to look at the definition of Callback.  Using the same ``grep``
  trick as we used to find ``TracedCallback``, you will be able to find
  that the file ``./core/callback.h`` is the one we need to look at.

Μπορείτε να δείτε τώρα την εφαρμογή του όλα όσα έχουμε μιλήσει. Ο κώδικας δημιουργεί μία επανάκληση του σωστού τύπου και 
αναθέτει τη συνάρτησή σας σε αυτό. Αυτό είναι το ισοδύναμο του ``pfi = MyFunction`` που συζητήσαμε στην αρχή του παρόντος 
τμήματος. Ο κώδικας στη συνέχεια προσθέτει την Επανάκληση στην λίστα των Επανακλήσεων για αυτήν την πηγή. Το μόνο που μένει 
είναι να δούμε τον ορισμό της Επανάκλησης. Χρησιμοποιώντας το ίδιο τέχνασμα ``grep`` όπως συνηθίζαμε να βρούμε ``TracedCallback``, 
θα είστε σε θέση να βρείτε ότι το αρχείο ``./core/callback.h`` είναι αυτό που πρέπει να εξετάσουμε.

..
  If you look down through the file, you will see a lot of probably
  almost incomprehensible template code.  You will eventually come to
  some API Documentation for the Callback template class, though.  Fortunately,
  there is some English:

Αν κοιτάξετε κάτω από το αρχείο, θα δείτε μια πολύ πιθανώς σχεδόν ακατανόητο κώδικα προτύπου. Εσείς τελικά θα καταλήξετε σε 
κάποια API Τεκμηρίωση για την κλάση Επανάκλησης πρότυπο, όμως. Ευτυχώς, υπάρχουν κάποια αγγλικά:

  **Callback** template class.

  This class template implements the Functor Design Pattern. It is used to declare the type of a **Callback**:

  * the first non-optional template argument represents the return type of the callback.
  * the reminaining (optional) template arguments represent the type of the subsequent arguments to the callback.
  * up to nine arguments are supported.

..
  We are trying to figure out what the

Προσπαθούμε να καταλάβουμε τι
::

    Callback<void, Ptr<const MobilityModel> > cb;

..
  declaration means.  Now we are in a position to understand that the
  first (non-optional) template argument, ``void``, represents the
  return type of the Callback.  The second (optional) template argument,
  ``Ptr<const MobilityModel>`` represents the type of the first argument
  to the callback.

σημαίνει η δήλωση. Τώρα είμαστε σε θέση να καταλάβουμε ότι το πρώτο (μη προαιρετικό) πρότυπο όρισμα , ``void``, 
αντιπροσωπεύει τον τύπο επιστροφής της Επανάκλησης. Το δεύτερο (προαιρετικά) πρότυπο όρισμα, ``Ptr<const MobilityModel>`` 
αντιπροσωπεύει τον τύπο του πρώτου ορίσματος επανάκλησης.

..
  The Callback in question is your function to receive the trace events.
  From this you can infer that you need a function that returns ``void``
  and takes a ``Ptr<const MobilityModel>``.  For example,

Η Επανάκληση σε ερώτηση είναι η συνάρτησή σας για να λαμβάνετε τα γεγονότα ίχνους. Από αυτό μπορείτε να συμπεράνετε ότι 
χρειάζεστε μια συνάρτηση που επιστρέφει ``void`` και παίρνει ένα ``Ptr<const MobilityModel>``. Για παράδειγμα,
::

  void
  CourseChangeCallback (Ptr<const MobilityModel> model)
  {
    ...
  }

..
  That's all you need if you want to ``Config::ConnectWithoutContext``.
  If you want a context, you need to ``Config::Connect`` and use a
  Callback function that takes a string context.  This is because the
  ``Connect`` function will provide the context for you.  You'll need

Αυτό είναι το μόνο που χρειάζεστε, αν θέλετε να ``Config::ConnectWithoutContext``. Αν θέλετε ένα πλαίσιο, θα πρέπει να 
``Config::Connect`` και να χρησιμοποιήσετε μια συνάρτηση Επανάκλησης που παίρνει ένα περιβάλλον συμβολοσειράς. Αυτό 
οφείλεται στο γεγονός ότι η συνάρτηση ``Connect`` θα παρέχει το περιεχόμενο για εσάς. Θα χρειαστείτε
::

  void
  CourseChangeCallback (std::string context, Ptr<const MobilityModel> model)
  {
    ...
  }

..
  If you want to ensure that your ``CourseChangeCallback`` is only
  visible in your local file, you can add the keyword ``static`` and
  come up with

Αν θέλετε να διασφαλίσετε ότι το ``CourseChangeCallback`` είναι ορατό μόνο σε τοπικό αρχείο σας, μπορείτε να προσθέσετε τη 
λέξη-κλειδί ``static`` και να καταλήξετε σε
::

  static void
  CourseChangeCallback (std::string path, Ptr<const MobilityModel> model)
  {
    ...
  }

..
  which is exactly what we used in the ``third.cc`` example.  Perhaps
  you should now go back and reread the previous section (Take My Word
  for It).

το οποίο είναι ακριβώς αυτό που χρησιμοποιήσαμε στο παράδειγμα ``third.cc``. Ίσως θα πρέπει να πάτε πίσω και να 
διαβάσετε πάλι την προηγούμενη ενότητα.

..
  If you are interested in more details regarding the implementation of
  Callbacks, feel free to take a look at the |ns3| manual.  They are one
  of the most frequently used constructs in the low-level parts of
  |ns3|.  It is, in my opinion, a quite elegant thing.

Εάν ενδιαφέρεστε για περισσότερες λεπτομέρειες σχετικά με την εφαρμογή των Επανακλήσεων, μη διστάσετε να ρίξετε μια ματιά 
στο manual του |ns3|. Αποτελούν ένα από τα πιο συχνά χρησιμοποιούμενα κατασκευάσματα στα τμήματα χαμηλού επιπέδου του 
|ns3|. Είναι, κατά τη γνώμη μου, ένα πολύ κομψό πράγμα.

..
  TracedValues

TracedValues
++++++++++++

..
  Earlier in this section, we presented a simple piece of code that used
  a ``TracedValue<int32_t>`` to demonstrate the basics of the tracing
  code.  We just glossed over the what a TracedValue really is and how
  to find the return type and formal arguments for the callback.

Νωρίτερα σε αυτή την ενότητα, παρουσιάσαμε ένα απλό κομμάτι του κώδικα που χρησιμοποιείται στο ``TracedValue<int32_t>`` να 
αποδείξει τα βασικά στοιχεία του κώδικα ανίχνευσης. Εμείς απλά προσπαθήσαμε να καταλάβουμε το τι είναι πραγματικά η TracedValue 
και πώς να βρείτε τον τύπο επιστροφής και τα επίσημα ορίσματα για την επανάκληση.

..
  As we mentioned, the file, ``traced-value.h`` brings in the required
  declarations for tracing of data that obeys value semantics.  In
  general, value semantics just means that you can pass the object
  itself around, rather than passing the address of the object.  We
  extend that requirement to include the full set of assignment-style
  operators that are pre-defined for plain-old-data (POD) types:

Όπως αναφέραμε, το αρχείο, ``traced-value.h`` φέρνει στις απαιτούμενες δηλώσεις για τον εντοπισμό των δεδομένων που υπακούει 
στη σημασιολογική αξία. Σε γενικές γραμμές, η σημασιολογική αξία απλά σημαίνει ότι μπορείτε να περάσετε το ίδιο το αντικείμενο 
γύρω του, αντί να περάσεις την διεύθυνση του αντικειμένου. Επεκτείνουμε την απαίτηση να συμπεριλάβουμε το σύνολο των φορέων 
ανάθεσης-στιλ που είναι προκαθορισμένα σε plain-old-data (POD) τύπους:

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

..
  What this all really means is that you will be able to trace all
  changes made using those operators to a C++ object which has value
  semantics.

Αυτό σημαίνει πραγματικά ότι θα είστε σε θέση να εντοπίζετε όλες τις αλλαγές που γίνονται με τη χρήση των εν λόγω φορέων σε 
αντικείμενο C++ που έχει σημασιολογική αξία.

..
  The ``TracedValue<>`` declaration we saw above provides the
  infrastructure that overloads the operators mentioned above and drives
  the callback process.  On use of any of the operators above with a
  ``TracedValue`` it will provide both the old and the new value of that
  variable, in this case an ``int32_t`` value.  By inspection of the
  ``TracedValue`` declaration, we know the trace sink function will have
  arguments ``(const int32_t oldValue, const int32_t newValue)``.  The
  return type for a ``TracedValue`` callback function is always
  ``void``, so the expected callback signature will be

Η δήλωση ``TracedValue<>`` που είδαμε παραπάνω παρέχει την υποδομή που επιβαρύνει τους φορείς που αναφέρονται ανωτέρω και 
κινεί τη διαδικασία επανάκλησης. Σχετικά με τη χρήση οποιουδήποτε από τα παραπάνω φορέων με ένα ``TracedValue`` θα παρέχει 
τόσο την παλιά όσο και τη νέα τιμή της μεταβλητής, σε αυτή την περίπτωση μία αξία ``int32_t``. Με την επιθεώρηση της δήλωσης 
``TracedValue``, γνωρίζουμε ότι η συνάρτηση της καταβόθρας ίχνους θα έχει ορίσματα ``(const int32_t oldValue, const int32_t newValue)``. 
Ο τύπος επιστροφής για μία συνάρτηση Επανάκλησης ``TracedValue`` είναι πάντα ``void``, έτσι ώστε το αναμενόμενο στίγμα Επανάκλησης θα είναι
::

  void (* TracedValueCallback)(const int32_t oldValue, const int32_t newValue);

..
  The ``.AddTraceSource`` in the ``GetTypeId`` method provides the
  "hooks" used for connecting the trace source to the outside world
  through the Config system.  We already discussed the first three
  agruments to ``AddTraceSource``: the Attribute name for the Config
  system, a help string, and the address of the TracedValue class data
  member.

Το ``.AddTraceSource`` στη μέθοδο ``GetTypeId`` παρέχει το "αγκίστρι" που χρησιμοποιείται για τη σύνδεση της πηγής ίχνους με 
τον έξω κόσμο μέσω του συστήματος Config. Έχουμε ήδη συζητήσει τα τρία πρώτα ορίσματα στο ``AddTraceSource``: το όνομα του 
Χαρακτηριστικού για το σύστημα Config, μια συμβολοσειρά βοήθεια, και τη διεύθυνση του μέλους κλάσης δεδομένων TracedValue.

..
  The final string argument, "ns3::Traced::Value::Int32" in the example,
  is the name of a ``typedef`` for the callback function signature.  We
  require these signatures to be defined, and give the fully qualified
  type name to ``AddTraceSource``, so the API documentation can link a
  trace source to the function signature.  For TracedValue the signature
  is straightforward; for TracedCallbacks we've already seen the API
  docs really help.

Το τελευταίο όρισμα συμβολοσειρών, στο παράδειγμα "ns3::Traced::Value::Int32", είναι το όνομα ενός ``typedef`` για το 
στίγμα της συνάρτησης επανάκλησης. Χρειαζόμαστε αυτές τις υπογραφές-στίγματα που θα καθοριστούν, και να δώσουμε το πλήρως 
αναγνωρισμένο όνομα τύπου στο ``AddTraceSource``, έτσι ώστε η τεκμηρίωση API μπορεί να συνδέσει μια πηγή ίχνους στο 
στίγμα της συνάρτησης. Για το στίγμα TracedValue είναι απλό, για TracedCallbacks έχουμε ήδη δει τα API Έγγραφα που πραγματικά 
βοηθούν.

..
  Real Example

Πραγματικό Παράδειγμα
*********************

..
  Let's do an example taken from one of the best-known books on TCP
  around.  "TCP/IP Illustrated, Volume 1: The Protocols," by W. Richard
  Stevens is a classic.  I just flipped the book open and ran across a
  nice plot of both the congestion window and sequence numbers versus
  time on page 366.  Stevens calls this, "Figure 21.10. Value of cwnd
  and send sequence number while data is being transmitted."  Let's just
  recreate the cwnd part of that plot in |ns3| using the tracing system
  and ``gnuplot``.

Ας κάνουμε ένα παράδειγμα από ένα βιβλίο, από τα πιο γνωστά βιβλία σχετικά με το πρωτόκολλο TCP γύρω. Είναι ένα κλασικό βιβλίο 
"TCP/IP Illustrated, Volume 1: The Protocols," από τον W. Richard Stevens. Απλά άφησα το βιβλίο ανοιχτό και έτρεξα ένα 
ωραίο σύνολο αριθμών τόσο το παράθυρο συμφόρησης όσο και τη σειρά συναρτήσει του χρόνου στη σελίδα 366. Ο Stevens το αποκαλεί 
αυτό "Figure 21.10. Value of cwnd and send sequence number while data is being transmitted." Ας δημιουργήσουμε πάλι το μέρος 
cwnd αυτού του συνόλου σε |ns3| με τη χρήση του συστήματος εντοπισμού και ``gnuplot``.

..
  Available Sources

Διαθέσιμες Πηγές
++++++++++++++++

..
  The first thing to think about is how we want to get the data out.
  What is it that we need to trace?  So let's consult "All Trace
  Sources" list to see what we have to work with.  Recall that this is
  found in the |ns3| API Documentation.  If you scroll through the list,
  you will eventually find:

Το πρώτο πράγμα που πρέπει να σκεφτούμε είναι το πώς θέλουμε να βγάλουμε τα στοιχεία έξω. Τι είναι αυτό που χρειαζόμαστε 
να εντοπίσουμε; Ας συμβουλευτούμε τη λίστα "All Trace Sources" για να δούμε με τι θα εργαστούμε. Θυμηθείτε ότι αυτό 
βρίσκεται στο |ns3| API Documentation. Αν μετακινηθείτε μέσα στη λίστα, θα βρείτε τελικά:

  **ns3::TcpNewReno**

  * **CongestionWindow**: The TCP connection's congestion window
  * **SlowStartThreshold**: TCP slow start threshold (bytes)

..
  It turns out that the |ns3| TCP implementation lives (mostly) in the
  file ``src/internet/model/tcp-socket-base.cc`` while congestion
  control variants are in files such as
  ``src/internet/model/tcp-newreno.cc``.  If you don't know this *a
  priori*, you can use the recursive ``grep`` trick:

Αποδεικνύεται ότι η |ns3| TCP implementation υπάρχει (κυρίως) στο αρχείο ``src/internet/model/tcp-socket-base.cc`` ενώ 
οι παραλλαγές ελέγχου συμφόρησης στα αρχεία, όπως ``src/internet/model/tcp-newreno.cc``. Αν δεν γνωρίζετε αυτό το *a priori*, 
μπορείτε να χρησιμοποιήσετε το αναδρομικό ``grep`` τέχνασμα:

.. sourcecode:: bash

  $ find . -name '*.cc' | xargs grep -i tcp

..
  You will find page after page of instances of tcp pointing you to that
  file.

Θα βρείτε τη σελίδα μετά τη σελίδα των περιεχομένων του TCP που δείχνουν προς αυτό το αρχείο.

..
  Bringing up the class documentation for ``TcpNewReno`` and skipping to
  the list of TraceSources you will find

Φέρνοντας την τεκμηρίωση της κλάσης για ``TcpNewReno`` και παρακάμπτοντας τη λίστα των TraceSources θα βρείτε

  **TraceSources**

  * **CongestionWindow**: The TCP connnection's congestion window

    Callback signature:  **ns3::Traced::Value::Uint322Callback**

..
  Clicking on the callback ``typedef`` link we see the signature
  you now know to expect

Κάνοντας κλικ στο σύνδεσμο επανάκλησης ``typedef`` βλέπουμε την υπογραφή και ξέρουμε τώρα να περιμένουμε
::

    typedef void(* ns3::Traced::Value::Int32Callback)(const int32_t oldValue, const int32_t newValue)

..
  You should now understand this code completely.  If we have a pointer
  to the ``TcpNewReno``, we can ``TraceConnect`` to the
  "CongestionWindow" trace source if we provide an appropriate callback
  target.  This is the same kind of trace source that we saw in the
  simple example at the start of this section, except that we are
  talking about ``uint32_t`` instead of ``int32_t``.  And we know
  that we have to provide a callback function with that signature.

Θα πρέπει να καταλάβετε τώρα αυτόν τον κώδικα εντελώς. Αν έχουμε ένα δείκτη προς το ``TcpNewReno``, μπορούμε να
``TraceConnect`` στη πηγή ίχνους "CongestionWindow" αν παρέχουμε τον κατάλληλο στόχο επανάκλησης. Αυτό είναι το ίδιο είδος 
της πηγής ίχνους που είδαμε στο απλό παράδειγμα κατά την έναρξη του παρόντος τμήματος, εκτός από το ότι μιλάμε για 
``uint32_t`` αντί ``int32_t``. Και ξέρουμε ότι πρέπει να παρέχουμε μια συνάρτηση επανάκλησης με αυτή την υπογραφή.

..
  Finding Examples

Βρίσκοντας Παραδείγματα
+++++++++++++++++++++++

..
  It's always best to try and find working code laying around that you
  can modify, rather than starting from scratch.  So the first order of
  business now is to find some code that already hooks the
  "CongestionWindow" trace source and see if we can modify it.  As
  usual, ``grep`` is your friend:

Είναι πάντα καλύτερο να προσπαθήσουμε να βρούμε τον κώδικα που δουλεύει, και μπορείτε να τροποποιήσετε, αντί να αρχίσετε από 
το μηδέν. Έτσι, η πρώτη σειρά των εργασιών είναι τώρα να βρείτε κάποιο κώδικα που ενώνεται ήδη στη πηγή ίχνους 
"CongestionWindow"  και να δούμε αν μπορούμε να το τροποποιήσουμε. Ως συνήθως, ο ``grep`` είναι ο φίλος σας:

.. sourcecode:: bash

  $ find . -name '*.cc' | xargs grep CongestionWindow

..
  This will point out a couple of promising candidates: 
  ``examples/tcp/tcp-large-transfer.cc`` and 
  ``src/test/ns3tcp/ns3tcp-cwnd-test-suite.cc``.

Θα επισημάνω μερικούς ελπιδοφόρους υποψηφίους: ``examples/tcp/tcp-large-transfer.cc`` και ``src/test/ns3tcp/ns3tcp-cwnd-test-suite.cc``.

..
  We haven't visited any of the test code yet, so let's take a look
  there.  You will typically find that test code is fairly minimal, so
  this is probably a very good bet.  Open
  ``src/test/ns3tcp/ns3tcp-cwnd-test-suite.cc`` in your favorite editor
  and search for "CongestionWindow".  You will find,

Δεν έχουμε επισκεφθεί κάποιο κώδικα δοκιμής ακόμα, οπότε ας ρίξουμε μια ματιά εκεί. Θα βρείτε συνήθως ότι ο κώδικας της 
δοκιμής είναι αρκετά μηδαμινός, έτσι αυτό είναι ίσως ένα πολύ καλό στοίχημα. Ανοίξτε ``src/test/ns3tcp/ns3tcp-cwnd-test-suite.cc`` 
στο αγαπημένο σας επεξεργαστή και αναζητήστε για "CongestionWindow". Θα βρείτε,

::

  ns3TcpSocket->TraceConnectWithoutContext ("CongestionWindow", 
    MakeCallback (&Ns3TcpCwndTestCase1::CwndChange, this));

..
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

Αυτό θα έπρεπε να είναι γνωστό σε σας. ναφερθήκαμε παραπάνω ότι αν είχαμε ένα δείκτη στο ``TcpNewReno``, θα μπορούσαμε να 
``TraceConnect`` στη πηγή ίχνους "CongestionWindow". Αυτό είναι ακριβώς αυτό που έχουμε εδώ. Έτσι, αποδεικνύεται ότι αυτή η 
γραμμή του κώδικα κάνει ακριβώς αυτό που θέλουμε. Ας πάμε μπροστά και να εξάγουμε τον κώδικα που χρειαζόμαστε από τη συνάρτηση 
αυτή (``Ns3TcpCwndTestCase1::DoRun (void)``). Αν κοιτάξετε αυτή τη συνάρτηση, θα διαπιστώσετε ότι μοιάζει ακριβώς όπως ένα 
σενάριο |ns3|. Αποδεικνύεται ότι είναι ακριβώς αυτό που είναι. Πρόκειται για ένα σενάριο που εκτελείται από το πλαίσιο της 
δοκιμής, ώστε να μπορούμε να το τραβήξουμε ακριβώς έξω και να το τυλίξουμε σε ``main`` αντί για ``DoRun``. Αντί να 
περπατήσετε μέσα από αυτό, βήμα, βήμα, παρέχουμε το αρχείο που προκύπτει από το porting της δοκιμής πίσω 
σε ένα μητρικό σενάριο |ns3| -- ``examples/tutorial/fifth.cc``.

..
  Dynamic Trace Sources

Δυναμικές Πηγές Εντοπισμού
++++++++++++++++++++++++++

..
  The ``fifth.cc`` example demonstrates an extremely important rule that
  you must understand before using any kind of trace source: you must
  ensure that the target of a ``Config::Connect`` command exists before
  trying to use it.  This is no different than saying an object must be
  instantiated before trying to call it.  Although this may seem obvious
  when stated this way, it does trip up many people trying to use the
  system for the first time.

Το παράδειγμα `` fifth.cc`` καταδεικνύει ένα εξαιρετικά σημαντικό κανόνα που πρέπει να καταλάβετε πριν από τη χρήση κάθε 
είδους πηγή ίχνους: θα πρέπει να βεβαιωθείτε ότι ο στόχος της εντολής ``Config::Connect`` υπάρχει πριν προσπαθήσετε να το 
χρησιμοποιήσετε. Αυτό δεν είναι διαφορετικό από το να λέμε ένα αντικείμενο πρέπει να αρχικοποιείται πριν προσπαθήσετε να το 
καλέσετε. Αν και αυτό μπορεί να φαίνεται προφανές, όταν δηλώθηκε αυτός ο τρόπος, κάνοντας τρικλοποδιά σε πολλούς ανθρώπους 
που προσπαθούν να χρησιμοποιήσουν το σύστημα για πρώτη φορά.

..
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

Ας επιστρέψουμε στα βασικά για μια στιγμή. Υπάρχουν τρεις βασικές φάσεις εκτέλεσης που υπάρχουν σε κάθε σενάριο |ns3|. Η 
πρώτη φάση μερικές φορές ονομάζεται "Configuration Time" ή "Setup Time", και υπάρχει κατά τη διάρκεια της περιόδου, όταν η 
συνάρτηση ``main`` από το σενάριό σας τρέχει, αλλά πριν ο ``Simulator::Run`` έχει καλεστεί. Η δεύτερη φάση μερικές φορές 
ονομάζεται "Simulation Time" και υπάρχει κατά τη διάρκεια της χρονικής περιόδουπου είναι ενεργά εκτελέσιμα τα γεγονότα του 
``Simulator::Run``. Μετά την ολοκλήρωση της εκτέλεσης της προσομοίωσης, ``Simulator::Run`` θα επιστρέψει στον έλεγχο πίσω 
στη συνάρτηση ``main``. Όταν συμβαίνει αυτό, τα σενάρια εισέρχονται σε αυτό το τι μπορεί να ονομάζεται "Teardown Phase", η 
οποία είναι όταν οι κατασκευές και τα αντικείμενα που δημιουργήθηκαν κατά τη διάρκεια της εγκατάστασης ληφθούν χώρια 
και κυκλοφορήσουν.

..
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

Ίσως το πιο κοινό λάθος που γίνεται στην προσπάθειά τους να χρησιμοποιήσουν το σύστημα εντοπισμού υποθέτει ότι οι οντότητες 
που δημιουργούνται δυναμικά *κατά τη διάρκεια της προσομοίωσης* είναι διαθέσιμες κατά τη διάρκεια της ρύθμισης. Ειδικότερα, 
ένας ``Socket`` |ns3| είναι ένα δυναμικό αντικείμενο που δημιουργείται συχνά από ``Applications`` που επικοινωνούν μεταξύ 
``Nodes``. Ένα ``Application`` |ns3| έχει πάντα ένα "Start Time" και ένα "Stop Time" που συνδέονται με αυτό. Στη συντριπτική 
πλειονότητα των περιπτώσεων, ένα ``Application`` δεν θα επιχειρήσει να δημιουργήσει ένα δυναμικό αντικείμενο έως ότου 
η μέθοδός του ``StartApplication`` καλείται σε κάποιο "Start Time". Αυτό γίνεται για να εξασφαλιστεί ότι η προσομοίωση είναι 
πλήρως διαμορφωμένη πριν το App προσπαθήσει να κάνει κάτι (ό,τι θα συνέβαινε αν προσπαθούσε να συνδεθεί σε έναν κόμβο που δεν 
υπήρχε ακόμα κατά τη διάρκεια της ρύθμισης). Ως αποτέλεσμα, κατά τη διάρκεια της φάσης διαμόρφωσης δεν μπορείτε να συνδέσετε 
μια πηγή ίχνους σε μία καταβόθρα ίχνους αν μία από αυτές δημιουργείται δυναμικά κατά τη διάρκεια της προσομοίωσης.

..
  The two solutions to this connundrum are

Οι δύο λύσεις για αυτό το connundrum είναι

..
  #. Create a simulator event that is run after the dynamic object is
     created and hook the trace when that event is executed; or
  
#. Δημιουργήστε ένα συμβάν προσομοιωτή που εκτελείται μετά το δυναμικό αντικείμενο που έχει δημιουργηθεί και συνδέστε το ίχνος, 
όταν εκτελείται αυτό το γεγονός, ή

..
  #. Create the dynamic object at configuration time, hook it then, and
     give the object to the system to use during simulation time.
     
#. Δημιουργήστε το δυναμικό αντικείμενο κατά το χρόνο διαμόρφωσης, κρατήστε'το στη συνέχεια, και δώστε το αντικείμενο στο 
σύστημα για να το χρησιμοποιήσει κατά τη διάρκεια της προσομοίωσης.

..
  We took the second approach in the ``fifth.cc`` example.  This
  decision required us to create the ``MyApp`` ``Application``, the
  entire purpose of which is to take a ``Socket`` as a parameter.

Πήραμε τη δεύτερη προσέγγιση στο παράδειγμα ``fifth.cc``. Αυτή η απόφαση μας απαίτησε να δημιουργήσουμε το ``MyApp`` ``Application``, 
ο ολόκληρος σκοπός της οποίας είναι να ρίξετε μια ``Socket`` ως παράμετρο.

..
  Walkthrough: ``fifth.cc``

Πέρασμα: ``fifth.cc``
+++++++++++++++++++++

..
  Now, let's take a look at the example program we constructed by
  dissecting the congestion window test.  Open
  ``examples/tutorial/fifth.cc`` in your favorite editor.  You should
  see some familiar looking code

Τώρα, ας ρίξουμε μια ματιά στο πρόγραμμα παράδειγμα που κατασκευάσαμε με ανατομή το τεστ παράθυρο συμφόρησης. Ανοίξτε το 
``examples/tutorial/fifth.cc`` στο αγαπημένο σας επεξεργαστή. Θα πρέπει να δείτε κάποιο γνωστό κώδικα
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

..
  This has all been covered, so we won't rehash it.  The next lines of
  source are the network illustration and a comment addressing the
  problem described above with ``Socket``.

Όλα αυτά έχουν καλυφθεί, γι 'αυτό δεν θα το αναμασήσουμε. Οι επόμενες γραμμές του κώδικα είναι η εικόνα του δικτύου και 
ένα σχόλιο αντιμετώπισης του προβλήματος που περιγράφηκε παραπάνω με το ``Socket``.
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

..
  This should also be self-explanatory.  

Αυτό θα πρέπει επίσης να είναι αυτονόητο.

..
  The next part is the declaration of the ``MyApp`` ``Application`` that
  we put together to allow the ``Socket`` to be created at configuration
  time.

Το επόμενο μέρος είναι η δήλωση του ``MyApp`` ``Application`` που έχουμε βάλει μαζί για να καταστεί δυνατή η ``Socket`` για να 
δημιουργηθούν κατά το χρόνο διαμόρφωσης.
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

..
  You can see that this class inherits from the |ns3| ``Application``
  class.  Take a look at ``src/network/model/application.h`` if you are
  interested in what is inherited.  The ``MyApp`` class is obligated to
  override the ``StartApplication`` and ``StopApplication`` methods.
  These methods are automatically called when ``MyApp`` is required to
  start and stop sending data during the simulation.

..
  Starting/Stopping Applications

Ξεκινώντας/Σταματώντας Εφαρμογών
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
  It is worthwhile to spend a bit of time explaining how events actually
  get started in the system.  This is another fairly deep explanation,
  and can be ignored if you aren't planning on venturing down into the
  guts of the system.  It is useful, however, in that the discussion
  touches on how some very important parts of |ns3| work and exposes
  some important idioms.  If you are planning on implementing new
  models you probably want to understand this section.

Αξίζει τον κόπο να περάσετε λίγο χρόνο εξηγώντας πώς τα γεγονότα πραγματικά ξεκίνησαν στο σύστημα. Αυτή είναι μία άλλη 
αρκετά βαθιά εξήγηση, και μπορεί να αγνοηθεί αν δεν κάνετε σχεδιασμό για τα εγχειρήματα μέσα στα σπλάχνα του συστήματος. 
Είναι χρήσιμο, ωστόσο, ότι η συζήτηση αγγίζει σχετικά με το πώς ορισμένα πολύ σημαντικά μέρη εργασίας στον |ns3| και 
εκθέτει κάποια σημαντικά ιδιώματα. Εάν σχεδιάζετε για την εφαρμογή νέων μοντέλων ίσως πρέπει να καταλάβετε αυτή την ενότητα.

..
  The most common way to start pumping events is to start an
  ``Application``.  This is done as the result of the following
  (hopefully) familar lines of an |ns3| script
  
Ο πιο συνηθισμένος τρόπος για να ξεκινήσετε την άντληση των γεγονότων είναι να ξεκινήσει μια ``Application``. Αυτό γίνεται 
ως αποτέλεσμα των ακόλουθων (ελπίζουμε) οικείων γραμμών του σεναρίου |ns3|
::

  ApplicationContainer apps = ...
  apps.Start (Seconds (1.0));
  apps.Stop (Seconds (10.0));

..
  The application container code (see
  ``src/network/helper/application-container.h`` if you are interested)
  loops through its contained applications and calls,

Ο κώδικας της εφαρμογής (βλέπε ``src/network/helper/application-container.h`` αν σας ενδιαφέρει) κάνει επαναλήψεις μέσω 
των εφαρμογών και των κλήσεών του,
::

  app->SetStartTime (startTime);

..
  as a result of the ``apps.Start`` call and

ως αποτέλεσμα της κλήσης ``apps.Start`` και
::

  app->SetStopTime (stopTime);

..
  as a result of the ``apps.Stop`` call.

ως αποτέλεσμα της κλήσης ``apps.Stop``.

..
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

Το τελικό αποτέλεσμα αυτών των κλήσεων είναι ότι θέλουμε να έχουμε τον προσομοιωτή να κάνει αυτόματα τις κλήσεις στις 
``Applications`` για να τους πεί πότε θα ξεκινήσουν και να σταματήσουν. Στην περίπτωση της ``MyApp``, κληρονομεί από την 
κλάση ``Application`` και παρακάμπτει ``StartApplication``, και ``StopApplication``. Αυτές είναι οι συναρτήσεις που θα 
κληθούν από τον προσομοιωτή την κατάλληλη στιγμή. Στην περίπτωση της ``MyApp`` θα διαπιστώσετε ότι η ``MyApp::StartApplication`` 
κάνει την αρχική ``Bind``, και ``Connect`` στην υποδοχή και στη συνέχεια ξεκινά η ροή δεδομένων καλώντας την ``MyApp::SendPacket``. 
Η ``MyApp::StopApplication`` σταματάει να παράγει πακέτα, ακυρώνοντας κάθε γεγονώς που εκκρεμεί και στη συνέχεια κλείνει την υποδοχή.

..
  One of the nice things about |ns3| is that you can completely ignore
  the implementation details of how your ``Application`` is
  "automagically" called by the simulator at the correct time.  But
  since we have already ventured deep into |ns3| already, let's go for
  it.

Ένα από τα ωραία πράγματα για τον |ns3| είναι ότι μπορείτε να αγνοήσετε εντελώς τις λεπτομέρειες της εφαρμογής του πώς το 
``Application`` σας "automagically" καλείται από τον προσομοιωτή στο σωστό χρόνο. Αλλά δεδομένου ότι έχουμε ήδη αποτολμήσει 
βαθιά μέσα στον |ns3| ήδη, ας πάμε για αυτό.

..
  If you look at ``src/network/model/application.cc`` you will find that
  the ``SetStartTime`` method of an ``Application`` just sets the member
  variable ``m_startTime`` and the ``SetStopTime`` method just sets
  ``m_stopTime``.  From there, without some hints, the trail will
  probably end.

Αν κοιτάξετε στο ``src/network/model/application.cc`` θα διαπιστώσετε ότι η μέθοδος ``SetStartTime`` από ένα ``Application`` 
θέτει μόνο τη μεταβλητή μέλος ``m_startTime`` και τη μέθοδο ``SetStopTime`` καθορίζει ακριβώς το ``m_stopTime``. Από εκεί, 
χωρίς κάποιες συμβουλές, η διαδρομή τελειώνει.

..
  The key to picking up the trail again is to know that there is a
  global list of all of the nodes in the system.  Whenever you create a
  node in a simulation, a pointer to that Node is added to the global
  ``NodeList``.

Το κλειδί για να πάρει το μονοπάτι ξανά είναι να γνωρίζουμε ότι υπάρχει μια παγκόσμια λίστα με όλους τους κόμβους του 
συστήματος. Κάθε φορά που δημιουργείτε ένα κόμβο σε μια προσομοίωση, ένας δείκτης σε αυτόν τον κόμβο προστίθεται στο 
παγκόσμιο ``NodeList``.

..
  Take a look at ``src/network/model/node-list.cc`` and search for
  ``NodeList::Add``.  The public static implementation calls into a
  private implementation called ``NodeListPriv::Add``.  This is a
  relatively common idom in |ns3|.  So, take a look at
  ``NodeListPriv::Add``.  There you will find,

Ρίξτε μια ματιά σε αυτό ``src/network/model/node-list.cc`` και ψάξτε για αυτό ``NodeList::Add``. Η δημόσια στατική εφαρμογή 
καλεί σε μια ιδιωτική εφαρμογή που ονομάζεται ``NodeListPriv::Add``. Αυτό είναι ένα σχετικά κοινό idom στον |ns3|. Έτσι, 
ρίξτε μια ματιά στο ``NodeListPriv::Add``. Εκεί θα βρείτε,
::

  Simulator::ScheduleWithContext (index, TimeStep (0), &Node::Initialize, node);

..
  This tells you that whenever a Node is created in a simulation, as
  a side-effect, a call to that node's ``Initialize`` method is
  scheduled for you that happens at time zero.  Don't read too much into
  that name, yet.  It doesn't mean that the Node is going to start doing
  anything, it can be interpreted as an informational call into the
  Node telling it that the simulation has started, not a call for
  action telling the Node to start doing something.

Αυτό σας λέει ότι κάθε φορά που ένας Κόμβος δημιουργείται σε μια προσομοίωση, ως παρενέργεια, μια κλήση στη μέθοδο του κόμβου 
``Initialize`` έχει προγραμματιστεί για εσάς που συμβαίνει σε χρόνο μηδέν. Μην διαβάζετε πάρα πολύ σε αυτό το όνομα, ακόμα. 
Αυτό δεν σημαίνει ότι ο Κόμβος πρόκειται να αρχίσει να κάνει κάτι, κι αυτό μπορεί να ερμηνευθεί ως μια ενημερωτική κλήση 
στον Κόμβο λέγοντάς του ότι η προσομοίωση έχει ξεκινήσει, δεν είναι μια κλήση για δράση λέγοντας ο κόμβος να αρχίσει να 
κάνει κάτι.

..
  So, ``NodeList::Add`` indirectly schedules a call to
  ``Node::Initialize`` at time zero to advise a new Node that the
  simulation has started.  If you look in ``src/network/model/node.h``
  you will, however, not find a method called ``Node::Initialize``.  It
  turns out that the ``Initialize`` method is inherited from class
  ``Object``.  All objects in the system can be notified when the
  simulation starts, and objects of class Node are just one kind of
  those objects.

Έτσι, το ``NodeList::Add`` έμμεσα προγραμματίζει μια κλήση στο ``Node::Initialize`` σε χρόνο μηδέν για να συμβουλεύσει ένα 
νέο Κόμβο που η προσομοίωση έχει ξεκινήσει. Αν κοιτάξετε στο ``src/network/model/node.h``δε θα βρείτε μια μέθοδο που 
ονομάζεται ``Node::Initialize``. Αποδεικνύεται ότι η μέθοδος ``Initialize`` κληρονομείται από την κλάση ``Object``. Όλα τα 
αντικείμενα του συστήματος μπορεί να ενημερώνονται όταν αρχίζει η προσομοίωση, και τα αντικείμενα της κλάσης Node είναι 
μόνο ένα είδος αυτών των αντικειμένων.

..
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

Ρίξτε μια ματιά εδώ ``src/core/model/object.cc`` και αναζητήστε για ``Object::Initialize``. Αυτός ο κώδικας δεν είναι τόσο 
απλός όσο αναμένεται τη στιγμή που ο |ns3| ``Objects`` υποστηρίζει τη συσσωμάτωση. Ο κώδικας στη ``Object::Initialize`` 
κάνει επαναλήψεις και στη συνέχεια μέσω όλων των αντικειμένων που έχουν συγκεντρωτικά μαζί και καλεί τη μέθοδο ``DoInitialize``. 
Αυτό είναι άλλο ένα ιδίωμα που είναι πολύ συχνό στον |ns3|, μερικές φορές ονομάζεται "template design pattern.": μια δημόσια 
μέθοδο μη-εικονική API, η οποία παραμένει σταθερή σε όλες τις εφαρμογές, και καλεί μια ιδιωτική μέθοδο εικονικής εφαρμογής 
που κληρονόμησε και έχει εφαρμοστεί απο υποκατηγορίες. Τα ονόματα είναι συνήθως κάτι σαν ``MethodName`` για το κοινό API 
και ``DoMethodName`` για το ιδιωτικό API.

..
  This tells us that we should look for a ``Node::DoInitialize`` method
  in ``src/network/model/node.cc`` for the method that will continue our
  trail.  If you locate the code, you will find a method that loops
  through all of the devices in the Node and then all of the
  applications in the Node calling ``device->Initialize`` and
  ``application->Initialize`` respectively.

Αυτό μας λέει ότι πρέπει να κοιτάξουμε για μια μέθοδο ``Node::DoInitialize`` στο ``src/network/model/node.cc`` για τη 
μέθοδο που θα συνεχίσει το μονοπάτι μας. Εάν εντοπίσετε τον κώδικα, θα βρείτε μια μέθοδο που κάνει επαναλήψεις μέσα από 
όλες τις συσκευές στον κόμβο και στη συνέχεια όλες τις εφαρμογές στον κόμβο καλώντας ``device->Initialize`` και 
``application->Initialize`` αντίστοιχα.

..
  You may already know that classes ``Device`` and ``Application`` both
  inherit from class ``Object`` and so the next step will be to look at
  what happens when ``Application::DoInitialize`` is called.  Take a
  look at ``src/network/model/application.cc`` and you will find

Ίσως γνωρίζετε ήδη ότι οι κλάσεις ``Device`` και ``Application`` κληρονομούν από την κλάση ``Object`` και 
έτσι το επόμενο βήμα θα είναι να εξετάσουμε τι συμβαίνει όταν καλείται το ``Application::DoInitialize``. Ρίξτε μια ματιά σε 
``src/network/model/application.cc`` και θα βρείτε
::

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

..
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

Εδώ,ερχόμαστε τελικά στο τέλος της διαδρομής. Αν το έχετε κρατήσει όλο "ευθεία", όταν εφαρμόζετε ένα |ns3| ``Application``, 
η νέα εφαρμογή σας κληρονομεί από την κλάση ``Application``. Μπορείτε να παρακάμψετε τις μεθόδους ``StartApplication`` και 
``StopApplication`` και παρέχει μηχανισμούς για την εκκίνηση και τη διακοπή της ροής των δεδομένων από το νέο ``Application`` σας. 
Όταν ένας Κόμβος δημιουργείται στην προσομοίωση, προστίθεται σε ένα παγκόσμιο ``NodeList``. Η πράξη της προσθήκης ενός Κόμβου 
σε αυτό το ``NodeList`` προκαλεί ένα γεγονώς του προσομοιωτή που έχει προγραμματιστεί για το χρόνο μηδέν το οποίο καλεί την 
μέθοδο ``Node::Initialize`` από το νέο προστιθέμενο κόμβο που θα καλείται όταν ξεκινά η προσομοίωση. Δεδομένου ότι ο Κόμβος 
κληρονομεί από ``Object``, αυτό καλεί τη μέθοδο ``Object::Initialize`` στον κόμβο που, με τη σειρά του, καλεί τις μεθόδους 
``DoInitialize`` για όλα τα ``Objects`` που συγκεντρώνονται σε κόμβους (σκεφτείτε μοντέλα κινητικότητας). Δεδομένου ότι ο 
Κόμβος ``Object`` έχει παρακαμφθεί το ``DoInitialize``, η μέθοδος καλείται όταν ξεκινά η προσομοίωση. Η μέθοδος ``Node::DoInitialize`` 
καλεί τις μεθόδους ``Initialize`` όλων των ``Applications`` στον κόμβο. Δεδομένου ότι τα ``Applications`` είναι επίσης 
``Objects``, αυτό προκαλεί να καλείται το ``Application::DoInitialize``. Όταν καλείται το ``Application::DoInitialize``, 
προγραμματίζει τα γεγονότα για το ``StartApplication`` και το ``StopApplication`` καλεί την ``Application``. Αυτές οι 
κλήσεις έχουν σχεδιαστεί για να ξεκινήσει και να σταματήσει τη ροή των δεδομένων από τις ``Application``

..
  This has been another fairly long journey, but it only has to be made
  once, and you now understand another very deep piece of |ns3|.

Αυτό ήταν ένα άλλο αρκετά μακρύ ταξίδι, αλλά χρειάζεται να γίνει μια φορά, και καταλαβαίνετε τώρα ένα άλλο πολύ βαθύ 
κομμάτι του |ns3|.

..
  The MyApp Application

Η Εφαρμογή MyApp
~~~~~~~~~~~~~~~~

..
  The ``MyApp`` ``Application`` needs a constructor and a destructor, of
  course

Το ``MyApp`` ``Application`` χρειάζεται έναν κατασκευαστή και καταστροφέα, φυσικά
::

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

..
  The existence of the next bit of code is the whole reason why we wrote
  this ``Application`` in the first place.

Η ύπαρξη του επόμενου κομματιού του κώδικα είναι ολόκληρος λόγος για τον οποίο γράψαμε αυτό το 
``Application`` στην πρώτη θέση.
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
  
..
  This code should be pretty self-explanatory.  We are just initializing
  member variables.  The important one from the perspective of tracing
  is the ``Ptr<Socket> socket`` which we needed to provide to the
  application during configuration time.  Recall that we are going to
  create the ``Socket`` as a ``TcpSocket`` (which is implemented by
  ``TcpNewReno``) and hook its "CongestionWindow" trace source before
  passing it to the ``Setup`` method.

Αυτός ο κώδικας θα πρέπει να είναι αρκετά αυτονόητος. Κάνουμε αρχικοποίηση μεταβλητών μέλους. Το σημαντικό από την άποψη 
του εντοπισμού είναι η ``Ptr<Socket> socket`` που χρειαζόμασταν για να παρέχει στην εφαρμογή κατά τη διάρκεια της ρύθμισης. 
Υπενθυμίζουμε ότι πρόκειται να δημιουργήσουμε το ``Socket`` ως ``TcpSocket`` (το οποίο υλοποιείται από το ``TcpNewReno``) 
και πιάνεται απο την πηγή ίχνους του "CongestionWindow" πριν από τη διοχέτευση προς την μέθοδο ``Setup``.
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

..
  The above code is the overridden implementation
  ``Application::StartApplication`` that will be automatically called by
  the simulator to start our ``Application`` running at the appropriate
  time. You can see that it does a ``Socket`` ``Bind`` operation.  If
  you are familiar with Berkeley Sockets this shouldn't be a surprise.
  It performs the required work on the local side of the connection just
  as you might expect.  The following ``Connect`` will do what is
  required to establish a connection with the TCP at ``Address`` m_peer.
  It should now be clear why we need to defer a lot of this to
  simulation time, since the ``Connect`` is going to need a fully
  functioning network to complete.  After the ``Connect``, the
  ``Application`` then starts creating simulation events by calling
  ``SendPacket``.

Ο παραπάνω κώδικας είναι ο προσπελάσιμος της εφαρμογή ``Application::StartApplication`` που θα κληθεί αυτόματα από τον 
προσομοιωτή για να ξεκινήσει η ``Application`` να τρέχει την κατάλληλη στιγμή. Μπορείτε να δείτε ότι κάνει μία λειτουργία ``Socket`` 
``Bind``. Εάν είστε εξοικειωμένοι με Berkeley Sockets αυτό δεν πρέπει να αποτελεί έκπληξη. Εκτελεί τις απαιτούμενες εργασίες 
για την τοπική πλευρά της σύνδεσης ακριβώς όπως μπορείτε να φανταστείτε. Το ακόλουθο ``Connect`` θα κάνει ό,τι χρειάζεται 
για να δημιουργήσει μια σύνδεση με το TCP σε ``Address`` m_peer. Θα πρέπει τώρα να είναι σαφές για ποιό λόγο θα πρέπει να 
αναβάλει πολύ αυτό το χρόνο προσομοίωσης, αφού ο ``Connect`` θα χρειαστεί ένα πλήρως λειτουργικό δίκτυο για να ολοκληρωθεί. 
Μετά τον ``Connect``, η ``Application`` ξεκινά στη συνέχεια τη δημιουργία γεγονότων προσομοίωσης καλώντας ``SendPacket``.

..
  The next bit of code explains to the ``Application`` how to stop
  creating simulation events.

Το επόμενο κομμάτι του κώδικα εξηγεί στο ``Application`` πώς να σταματήσουμε να δημιουργούμε γεγονότα προσομοίωσης.
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

..
  Every time a simulation event is scheduled, an ``Event`` is created.
  If the ``Event`` is pending execution or executing, its method
  ``IsRunning`` will return ``true``.  In this code, if ``IsRunning()``
  returns true, we ``Cancel`` the event which removes it from the
  simulator event queue.  By doing this, we break the chain of events
  that the ``Application`` is using to keep sending its ``Packets`` and
  the ``Application`` goes quiet.  After we quiet the ``Application`` we
  ``Close`` the socket which tears down the TCP connection.

Κάθε φορά που μια εκδήλωση προσομοίωσης έχει προγραμματιστεί, ένα ``Event`` δημιουργείται. Αν το ``Event`` εκκρεμεί η 
εκτέλεσή του, η μέθοδος της ``IsRunning`` θα επιστρέψει ``true``. Σε αυτόν τον κώδικα, αν ``IsRunning()`` επιστρέφει true, 
εμείς ``Cancel`` το γεγονός που αφαιρεί από την ουρά του προσομοιωτή εκδήλωσης. Με τον τρόπο αυτό, θα σπάσει την αλυσίδα 
των γεγονότων που η ``Application`` χρησιμοποιεί για να κρατήσει την αποστολή ``Packets`` και η ``Application`` πηγαίνει 
ήσυχη. Αφού ηρεμήσει η ``Application`` εμείς ``Close`` την υποδοχή που κατεδαφίζει τη σύνδεση TCP.

..
  The socket is actually deleted in the destructor when the ``m_socket =
  0`` is executed.  This removes the last reference to the underlying
  Ptr<Socket> which causes the destructor of that Object to be called.

Η υποδοχή είναι στην πραγματικότητα διαγραμένη από τον καταστροφέα, όταν η ``m_socket = 0`` εκτελείται. Αυτό αφαιρεί την 
τελευταία αναφορά στην υποκείμενη Ptr<Socket> που προκαλεί τον καταστροφέα του αντικειμένου που πρόκειται να κληθεί.

..
  Recall that ``StartApplication`` called ``SendPacket`` to start the
  chain of events that describes the ``Application`` behavior.

Υπενθυμίζουμε ότι ``StartApplication`` κάλεσε ``SendPacket`` για να ξεκινήσει η αλυσίδα των γεγονότων που περιγράφει τη συμπεριφορά 
``Application``.
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

..
  Here, you see that ``SendPacket`` does just that.  It creates a
  ``Packet`` and then does a ``Send`` which, if you know Berkeley
  Sockets, is probably just what you expected to see.

Εδώ, μπορείτε να δείτε ότι ``SendPacket`` κάνει ακριβώς αυτό. Δημιουργεί ένα ``Packet`` και στη συνέχεια κάνει ένα ``Send`` 
η οποία, αν ξέρετε Berkeley Sockets, είναι πιθανώς ακριβώς αυτό που περιμένατε να δείτε.

..
  It is the responsibility of the ``Application`` to keep scheduling the
  chain of events, so the next lines call ``ScheduleTx`` to schedule
  another transmit event (a ``SendPacket``) until the ``Application``
  decides it has sent enough.

Είναι ευθύνη του ``Application`` να κρατήσει τον προγραμματισμό της αλυσίδας των γεγονότων, έτσι ώστε οι επόμενες γραμμές 
καλούν τη ``ScheduleTx`` να προγραμματίσει μια άλλη περίπτωση μετάδοσης (μία ``SendPacket``) μέχρι το ``Application`` 
αποφασίσει ότι έχει στείλει αρκετά.
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

..
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

Εδώ, μπορείτε να δείτε ότι το ``ScheduleTx`` κάνει ακριβώς αυτό. Εάν η ``Application`` τρέχει (αν το ``StopApplication`` δεν 
έχει κληθεί) θα προγραμματίσετε μια νέα εκδήλωση, η οποία καλεί ``SendPacket`` ξανά. Ο αναγνώστης προειδοποίησης θα 
εντοπίσει κάτι που σκοντάφτουν και νέοι χρήστες. Ο ρυθμός δεδομένων ενός ``Application`` είναι ακριβώς αυτός. Δεν έχει 
τίποτε να κάνει με το ρυθμό δεδομένων ενός υποκείμενου ``Channel``. Αυτός είναι ο ρυθμός με τον οποίο η ``Application`` 
παράγει κομμάτια(bits). Δεν λαμβάνει υπόψη οποιαδήποτε επιβάρυνση για τα διάφορα πρωτόκολλα ή τα κανάλια που χρησιμοποιεί 
για τη μεταφορά των δεδομένων. Εάν ορίσετε το ρυθμό δεδομένων ενός ``Application`` στον ίδιο ρυθμό μετάδοσης δεδομένων ως 
υποκείμενο ``Channel`` θα υπερχειλίσει η μνήμης σας.

Trace Sinks
~~~~~~~~~~~

The whole point of this exercise is to get trace callbacks from TCP
indicating the congestion window has been updated.  The next piece of
code implements the corresponding trace sink::

  static void
  CwndChange (uint32_t oldCwnd, uint32_t newCwnd)
  {
    NS_LOG_UNCOND (Simulator::Now ().GetSeconds () << "\t" << newCwnd);
  }

This should be very familiar to you now, so we won't dwell on the
details.  This function just logs the current simulation time and the
new value of the congestion window every time it is changed.  You can
probably imagine that you could load the resulting output into a
graphics program (gnuplot or Excel) and immediately see a nice graph
of the congestion window behavior over time.

We added a new trace sink to show where packets are dropped.  We are
going to add an error model to this code also, so we wanted to
demonstrate this working.

::

  static void
  RxDrop (Ptr<const Packet> p)
  {
    NS_LOG_UNCOND ("RxDrop at " << Simulator::Now ().GetSeconds ());
  }

This trace sink will be connected to the "PhyRxDrop" trace source of
the point-to-point NetDevice.  This trace source fires when a packet
is dropped by the physical layer of a ``NetDevice``.  If you take a
small detour to the source
(``src/point-to-point/model/point-to-point-net-device.cc``) you will
see that this trace source refers to
``PointToPointNetDevice::m_phyRxDropTrace``.  If you then look in
``src/point-to-point/model/point-to-point-net-device.h`` for this
member variable, you will find that it is declared as a
``TracedCallback<Ptr<const Packet> >``.  This should tell you that the
callback target should be a function that returns void and takes a
single parameter which is a ``Ptr<const Packet>`` (assuming we use
``ConnectWithoutContext``) -- just what we have above.

Main Program
~~~~~~~~~~~~

The following code should be very familiar to you by now::

  int
  main (int argc, char *argv[])
  {
    NodeContainer nodes;
    nodes.Create (2);
  
    PointToPointHelper pointToPoint;
    pointToPoint.SetDeviceAttribute ("DataRate", StringValue ("5Mbps"));
    pointToPoint.SetChannelAttribute ("Delay", StringValue ("2ms"));
  
    NetDeviceContainer devices;
    devices = pointToPoint.Install (nodes);

This creates two nodes with a point-to-point channel between them,
just as shown in the illustration at the start of the file.

The next few lines of code show something new.  If we trace a
connection that behaves perfectly, we will end up with a monotonically
increasing congestion window.  To see any interesting behavior, we
really want to introduce link errors which will drop packets, cause
duplicate ACKs and trigger the more interesting behaviors of the
congestion window.

|ns3| provides ``ErrorModel`` objects which can be attached to
``Channels``.  We are using the ``RateErrorModel`` which allows us to
introduce errors
into a ``Channel`` at a given *rate*. 

::

  Ptr<RateErrorModel> em = CreateObject<RateErrorModel> ();
  em->SetAttribute ("ErrorRate", DoubleValue (0.00001));
  devices.Get (1)->SetAttribute ("ReceiveErrorModel", PointerValue (em));

The above code instantiates a ``RateErrorModel`` Object, and we set
the "ErrorRate" ``Attribute`` to the desired value.  We then set the
resulting instantiated ``RateErrorModel`` as the error model used by
the point-to-point ``NetDevice``.  This will give us some
retransmissions and make our plot a little more interesting.

::

  InternetStackHelper stack;
  stack.Install (nodes);

  Ipv4AddressHelper address;
  address.SetBase ("10.1.1.0", "255.255.255.252");
  Ipv4InterfaceContainer interfaces = address.Assign (devices);

The above code should be familiar.  It installs internet stacks on our
two nodes and creates interfaces and assigns IP addresses for the
point-to-point devices.

Since we are using TCP, we need something on the destination Node to
receive TCP connections and data.  The ``PacketSink`` ``Application``
is commonly used in |ns3| for that purpose.

::

  uint16_t sinkPort = 8080;
  Address sinkAddress (InetSocketAddress(interfaces.GetAddress (1), sinkPort));
  PacketSinkHelper packetSinkHelper ("ns3::TcpSocketFactory", 
    InetSocketAddress (Ipv4Address::GetAny (), sinkPort));
  ApplicationContainer sinkApps = packetSinkHelper.Install (nodes.Get (1));
  sinkApps.Start (Seconds (0.));
  sinkApps.Stop (Seconds (20.));

This should all be familiar, with the exception of,

::

  PacketSinkHelper packetSinkHelper ("ns3::TcpSocketFactory", 
    InetSocketAddress (Ipv4Address::GetAny (), sinkPort));

This code instantiates a ``PacketSinkHelper`` and tells it to create
sockets using the class ``ns3::TcpSocketFactory``.  This class
implements a design pattern called "object factory" which is a
commonly used mechanism for specifying a class used to create objects
in an abstract way.  Here, instead of having to create the objects
themselves, you provide the ``PacketSinkHelper`` a string that
specifies a ``TypeId`` string used to create an object which can then
be used, in turn, to create instances of the Objects created by the
factory.

The remaining parameter tells the ``Application`` which address and
port it should ``Bind`` to.

The next two lines of code will create the socket and connect the
trace source.

::

  Ptr<Socket> ns3TcpSocket = Socket::CreateSocket (nodes.Get (0), 
    TcpSocketFactory::GetTypeId ());
  ns3TcpSocket->TraceConnectWithoutContext ("CongestionWindow", 
    MakeCallback (&CwndChange));

The first statement calls the static member function
``Socket::CreateSocket`` and provides a Node and an explicit
``TypeId`` for the object factory used to create the socket.  This is
a slightly lower level call than the ``PacketSinkHelper`` call above,
and uses an explicit C++ type instead of one referred to by a string.
Otherwise, it is conceptually the same thing.

Once the ``TcpSocket`` is created and attached to the Node, we can
use ``TraceConnectWithoutContext`` to connect the CongestionWindow
trace source to our trace sink.

Recall that we coded an ``Application`` so we could take that
``Socket`` we just made (during configuration time) and use it in
simulation time.  We now have to instantiate that ``Application``.  We
didn't go to any trouble to create a helper to manage the
``Application`` so we are going to have to create and install it
"manually".  This is actually quite easy::

  Ptr<MyApp> app = CreateObject<MyApp> ();
  app->Setup (ns3TcpSocket, sinkAddress, 1040, 1000, DataRate ("1Mbps"));
  nodes.Get (0)->AddApplication (app);
  app->Start (Seconds (1.));
  app->Stop (Seconds (20.));

The first line creates an ``Object`` of type ``MyApp`` -- our
``Application``.  The second line tells the ``Application`` what
``Socket`` to use, what address to connect to, how much data to send
at each send event, how many send events to generate and the rate at
which to produce data from those events.

Next, we manually add the ``MyApp Application`` to the source Node and
explicitly call the ``Start`` and ``Stop`` methods on the
``Application`` to tell it when to start and stop doing its thing.

We need to actually do the connect from the receiver point-to-point
``NetDevice`` drop event to our ``RxDrop`` callback now.

::

  devices.Get (1)->TraceConnectWithoutContext("PhyRxDrop", MakeCallback (&RxDrop));

It should now be obvious that we are getting a reference to the
receiving ``Node NetDevice`` from its container and connecting the
trace source defined by the attribute "PhyRxDrop" on that device to
the trace sink ``RxDrop``.

Finally, we tell the simulator to override any ``Applications`` and
just stop processing events at 20 seconds into the simulation.

::

    Simulator::Stop (Seconds(20));
    Simulator::Run ();
    Simulator::Destroy ();

    return 0;
  }

Recall that as soon as ``Simulator::Run`` is called, configuration
time ends, and simulation time begins.  All of the work we
orchestrated by creating the ``Application`` and teaching it how to
connect and send data actually happens during this function call.

As soon as ``Simulator::Run`` returns, the simulation is complete and
we enter the teardown phase.  In this case, ``Simulator::Destroy``
takes care of the gory details and we just return a success code after
it completes.

Running ``fifth.cc``
++++++++++++++++++++

Since we have provided the file ``fifth.cc`` for you, if you have
built your distribution (in debug mode since it uses ``NS_LOG`` -- recall
that optimized builds optimize out ``NS_LOG``) it will be waiting for you
to run.

.. sourcecode:: bash

  $ ./waf --run fifth
  Waf: Entering directory `/home/craigdo/repos/ns-3-allinone-dev/ns-3-dev/build'
  Waf: Leaving directory `/home/craigdo/repos/ns-3-allinone-dev/ns-3-dev/build'
  'build' finished successfully (0.684s)
  1       536
  1.0093  1072
  1.01528 1608
  1.02167 2144
  ...
  1.11319 8040
  1.12151 8576
  1.12983 9112
  RxDrop at 1.13696
  ...

You can probably see immediately a downside of using prints of any
kind in your traces.  We get those extraneous waf messages printed all
over our interesting information along with those RxDrop messages.  We
will remedy that soon, but I'm sure you can't wait to see the results
of all of this work.  Let's redirect that output to a file called
``cwnd.dat``:

.. sourcecode:: bash

  $ ./waf --run fifth > cwnd.dat 2>&1

Now edit up "cwnd.dat" in your favorite editor and remove the waf
build status and drop lines, leaving only the traced data (you could
also comment out the ``TraceConnectWithoutContext("PhyRxDrop",
MakeCallback (&RxDrop));`` in the script to get rid of the drop prints
just as easily.

You can now run gnuplot (if you have it installed) and tell it to
generate some pretty pictures:

.. sourcecode:: bash

  $ gnuplot
  gnuplot> set terminal png size 640,480
  gnuplot> set output "cwnd.png"
  gnuplot> plot "cwnd.dat" using 1:2 title 'Congestion Window' with linespoints
  gnuplot> exit

You should now have a graph of the congestion window versus time
sitting in the file "cwnd.png" that looks like:

.. figure:: figures/cwnd.png

Using Mid-Level Helpers
+++++++++++++++++++++++

In the previous section, we showed how to hook a trace source and get
hopefully interesting information out of a simulation.  Perhaps you
will recall that we called logging to the standard output using
``std::cout`` a "blunt instrument" much earlier in this chapter.  We
also wrote about how it was a problem having to parse the log output
in order to isolate interesting information.  It may have occurred to
you that we just spent a lot of time implementing an example that
exhibits all of the problems we purport to fix with the |ns3| tracing
system!  You would be correct.  But, bear with us.  We're not done
yet.

One of the most important things we want to do is to is to have the
ability to easily control the amount of output coming out of the
simulation; and we also want to save those data to a file so we can
refer back to it later.  We can use the mid-level trace helpers
provided in |ns3| to do just that and complete the picture.

We provide a script that writes the cwnd change and drop events
developed in the example ``fifth.cc`` to disk in separate files.  The
cwnd changes are stored as a tab-separated ASCII file and the drop
events are stored in a PCAP file.  The changes to make this happen are
quite small.

Walkthrough: ``sixth.cc``
~~~~~~~~~~~~~~~~~~~~~~~~~

Let's take a look at the changes required to go from ``fifth.cc`` to
``sixth.cc``.  Open ``examples/tutorial/sixth.cc`` in your favorite
editor.  You can see the first change by searching for CwndChange.
You will find that we have changed the signatures for the trace sinks
and have added a single line to each sink that writes the traced
information to a stream representing a file.

::

  static void
  CwndChange (Ptr<OutputStreamWrapper> stream, uint32_t oldCwnd, uint32_t newCwnd)
  {
    NS_LOG_UNCOND (Simulator::Now ().GetSeconds () << "\t" << newCwnd);
    *stream->GetStream () << Simulator::Now ().GetSeconds () << "\t" << oldCwnd << "\t" << newCwnd << std::endl;
  }
  
  static void
  RxDrop (Ptr<PcapFileWrapper> file, Ptr<const Packet> p)
  {
    NS_LOG_UNCOND ("RxDrop at " << Simulator::Now ().GetSeconds ());
    file->Write(Simulator::Now(), p);
  }

We have added a "stream" parameter to the ``CwndChange`` trace sink.
This is an object that holds (keeps safely alive) a C++ output stream.
It turns out that this is a very simple object, but one that manages
lifetime issues for the stream and solves a problem that even
experienced C++ users run into.  It turns out that the copy
constructor for ``std::ostream`` is marked private.  This means that
``std::ostreams`` do not obey value semantics and cannot be used in
any mechanism that requires the stream to be copied.  This includes
the |ns3| callback system, which as you may recall, requires objects
that obey value semantics.  Further notice that we have added the
following line in the ``CwndChange`` trace sink implementation::

  *stream->GetStream () << Simulator::Now ().GetSeconds () << "\t" << oldCwnd << "\t" << newCwnd << std::endl;

This would be very familiar code if you replaced ``*stream->GetStream
()`` with ``std::cout``, as in::

  std::cout << Simulator::Now ().GetSeconds () << "\t" << oldCwnd << "\t" << newCwnd << std::endl;

This illustrates that the ``Ptr<OutputStreamWrapper>`` is really just
carrying around a ``std::ofstream`` for you, and you can use it here
like any other output stream.

A similar situation happens in ``RxDrop`` except that the object being
passed around (a ``Ptr<PcapFileWrapper>``) represents a PCAP file.
There is a one-liner in the trace sink to write a timestamp and the
contents of the packet being dropped to the PCAP file::

  file->Write(Simulator::Now(), p);

Of course, if we have objects representing the two files, we need to
create them somewhere and also cause them to be passed to the trace
sinks.  If you look in the ``main`` function, you will find new code
to do just that::

  AsciiTraceHelper asciiTraceHelper;
  Ptr<OutputStreamWrapper> stream = asciiTraceHelper.CreateFileStream ("sixth.cwnd");
  ns3TcpSocket->TraceConnectWithoutContext ("CongestionWindow", MakeBoundCallback (&CwndChange, stream));

  ...

  PcapHelper pcapHelper;
  Ptr<PcapFileWrapper> file = pcapHelper.CreateFile ("sixth.pcap", std::ios::out, PcapHelper::DLT_PPP);
  devices.Get (1)->TraceConnectWithoutContext("PhyRxDrop", MakeBoundCallback (&RxDrop, file));

In the first section of the code snippet above, we are creating the
ASCII trace file, creating an object responsible for managing it and
using a variant of the callback creation function to arrange for the
object to be passed to the sink.  Our ASCII trace helpers provide a
rich set of functions to make using text (ASCII) files easy.  We are
just going to illustrate the use of the file stream creation function
here.

The ``CreateFileStream`` function is basically going to instantiate
a ``std::ofstream`` object and create a new file (or truncate an existing
file).  This ``std::ofstream`` is packaged up in an |ns3| object for lifetime
management and copy constructor issue resolution.

We then take this |ns3| object representing the file and pass it to
``MakeBoundCallback()``.  This function creates a callback just like
``MakeCallback()``, but it "binds" a new value to the callback.  This
value is added as the first argument to the callback before it is called.

Essentially, ``MakeBoundCallback(&CwndChange, stream)`` causes the
trace source to add the additional "stream" parameter to the front of
the formal parameter list before invoking the callback.  This changes
the required signature of the ``CwndChange`` sink to match the one
shown above, which includes the "extra" parameter
``Ptr<OutputStreamWrapper> stream``.

In the second section of code in the snippet above, we instantiate a
``PcapHelper`` to do the same thing for our PCAP trace file that we
did with the ``AsciiTraceHelper``. The line of code,

::

  Ptr<PcapFileWrapper> file = pcapHelper.CreateFile ("sixth.pcap",
  "w", PcapHelper::DLT_PPP);

creates a PCAP file named "sixth.pcap" with file mode "w".  This means
that the new file is truncated (contents deleted) if an existing file
with that name is found.  The final parameter is the "data link type"
of the new PCAP file.  These are the same as the PCAP library data
link types defined in ``bpf.h`` if you are familar with PCAP.  In this
case, ``DLT_PPP`` indicates that the PCAP file is going to contain
packets prefixed with point to point headers.  This is true since the
packets are coming from our point-to-point device driver.  Other
common data link types are DLT_EN10MB (10 MB Ethernet) appropriate for
csma devices and DLT_IEEE802_11 (IEEE 802.11) appropriate for wifi
devices.  These are defined in ``src/network/helper/trace-helper.h``
if you are interested in seeing the list.  The entries in the list
match those in ``bpf.h`` but we duplicate them to avoid a PCAP source
dependence.

A |ns3| object representing the PCAP file is returned from
``CreateFile`` and used in a bound callback exactly as it was in the
ASCII case.

An important detour: It is important to notice that even though both
of these objects are declared in very similar ways,

::

  Ptr<PcapFileWrapper> file ...
  Ptr<OutputStreamWrapper> stream ...

The underlying objects are entirely different.  For example, the
``Ptr<PcapFileWrapper>`` is a smart pointer to an |ns3| Object that is
a fairly heavyweight thing that supports Attributes and is integrated
into the Config system.  The ``Ptr<OutputStreamWrapper>``, on the
other hand, is a smart pointer to a reference counted object that is a
very lightweight thing.  Remember to look at the object you are
referencing before making any assumptions about the "powers" that
object may have.

For example, take a look at ``src/network/utils/pcap-file-wrapper.h``
in the distribution and notice,

::

  class PcapFileWrapper : public Object

that class ``PcapFileWrapper`` is an |ns3| Object by virtue of its
inheritance.  Then look at
``src/network/model/output-stream-wrapper.h`` and notice,

::

  class OutputStreamWrapper : public
  SimpleRefCount<OutputStreamWrapper>

that this object is not an |ns3| Object at all, it is "merely" a C++
object that happens to support intrusive reference counting.

The point here is that just because you read ``Ptr<something>`` it does
not necessarily mean that ``something`` is an |ns3| Object on which you
can hang |ns3| Attributes, for example.

Now, back to the example.  If you build and run this example,

.. sourcecode:: bash

  $ ./waf --run sixth

you will see the same messages appear as when you ran "fifth", but two
new files will appear in the top-level directory of your |ns3|
distribution.

.. sourcecode:: bash

  sixth.cwnd  sixth.pcap

Since "sixth.cwnd" is an ASCII text file, you can view it with ``cat``
or your favorite file viewer.

.. sourcecode:: bash

  1       0       536
  1.0093  536     1072
  1.01528 1072    1608
  1.02167 1608    2144
  ...
  9.69256 5149	  5204
  9.89311 5204	  5259

You have a tab separated file with a timestamp, an old congestion
window and a new congestion window suitable for directly importing
into your plot program.  There are no extraneous prints in the file,
no parsing or editing is required.

Since "sixth.pcap" is a PCAP file, you can fiew it with ``tcpdump``.

.. sourcecode:: bash

  reading from file sixth.pcap, link-type PPP (PPP)
  1.136956 IP 10.1.1.1.49153 > 10.1.1.2.8080: Flags [.], seq 17177:17681, ack 1, win 32768, options [TS val 1133 ecr 1127,eol], length 504
  1.403196 IP 10.1.1.1.49153 > 10.1.1.2.8080: Flags [.], seq 33280:33784, ack 1, win 32768, options [TS val 1399 ecr 1394,eol], length 504
  ...
  7.426220 IP 10.1.1.1.49153 > 10.1.1.2.8080: Flags [.], seq 785704:786240, ack 1, win 32768, options [TS val 7423 ecr 7421,eol], length 536
  9.630693 IP 10.1.1.1.49153 > 10.1.1.2.8080: Flags [.], seq 882688:883224, ack 1, win 32768, options [TS val 9620 ecr 9618,eol], length 536

You have a PCAP file with the packets that were dropped in the
simulation.  There are no other packets present in the file and there
is nothing else present to make life difficult.

It's been a long journey, but we are now at a point where we can
appreciate the |ns3| tracing system.  We have pulled important events
out of the middle of a TCP implementation and a device driver.  We
stored those events directly in files usable with commonly known
tools.  We did this without modifying any of the core code involved,
and we did this in only 18 lines of code::

  static void
  CwndChange (Ptr<OutputStreamWrapper> stream, uint32_t oldCwnd, uint32_t newCwnd)
  {
    NS_LOG_UNCOND (Simulator::Now ().GetSeconds () << "\t" << newCwnd);
    *stream->GetStream () << Simulator::Now ().GetSeconds () << "\t" << oldCwnd << "\t" << newCwnd << std::endl;
  }

  ...

  AsciiTraceHelper asciiTraceHelper;
  Ptr<OutputStreamWrapper> stream = asciiTraceHelper.CreateFileStream ("sixth.cwnd");
  ns3TcpSocket->TraceConnectWithoutContext ("CongestionWindow", MakeBoundCallback (&CwndChange, stream));

  ...

  static void
  RxDrop (Ptr<PcapFileWrapper> file, Ptr<const Packet> p)
  {
    NS_LOG_UNCOND ("RxDrop at " << Simulator::Now ().GetSeconds ());
    file->Write(Simulator::Now(), p);
  }

  ...
  
  PcapHelper pcapHelper;
  Ptr<PcapFileWrapper> file = pcapHelper.CreateFile ("sixth.pcap", "w", PcapHelper::DLT_PPP);
  devices.Get (1)->TraceConnectWithoutContext("PhyRxDrop", MakeBoundCallback (&RxDrop, file));

Trace Helpers
*************

The |ns3| trace helpers provide a rich environment for configuring and
selecting different trace events and writing them to files.  In
previous sections, primarily :ref:`BuildingTopologies`, we have seen
several varieties of the trace helper methods designed for use inside
other (device) helpers.

Perhaps you will recall seeing some of these variations: 

::

  pointToPoint.EnablePcapAll ("second");
  pointToPoint.EnablePcap ("second", p2pNodes.Get (0)->GetId (), 0);
  csma.EnablePcap ("third", csmaDevices.Get (0), true);
  pointToPoint.EnableAsciiAll (ascii.CreateFileStream ("myfirst.tr"));

What may not be obvious, though, is that there is a consistent model
for all of the trace-related methods found in the system.  We will now
take a little time and take a look at the "big picture".

There are currently two primary use cases of the tracing helpers in
|ns3|:  device helpers and protocol helpers.  Device helpers look at
the problem of specifying which traces should be enabled through a
(node, device) pair.  For example, you may want to specify that PCAP
tracing should be enabled on a particular device on a specific node.
This follows from the |ns3| device conceptual model, and also the
conceptual models of the various device helpers.  Following naturally
from this, the files created follow a ``<prefix>-<node>-<device>`` naming
convention.

Protocol helpers look at the problem of specifying which traces should
be enabled through a protocol and interface pair.  This follows from
the |ns3| protocol stack conceptual model, and also the conceptual
models of internet stack helpers.  Naturally, the trace files should
follow a ``<prefix>-<protocol>-<interface>`` naming convention.

The trace helpers therefore fall naturally into a two-dimensional
taxonomy.  There are subtleties that prevent all four classes from
behaving identically, but we do strive to make them all work as
similarly as possible; and whenever possible there are analogs for all
methods in all classes.

  +-----------------+---------+---------+
  |                 |  PCAP   |  ASCII  |
  +=================+=========+=========+
  | Device Helper   | |check| | |check| |
  +-----------------+---------+---------+
  | Protocol Helper | |check| | |check| |
  +-----------------+---------+---------+

We use an approach called a ``mixin`` to add tracing functionality to
our helper classes.  A ``mixin`` is a class that provides
functionality when it is inherited by a subclass.  Inheriting from a
mixin is not considered a form of specialization but is really a way
to collect functionality.

Let's take a quick look at all four of these cases and their
respective ``mixins``.

Device Helpers
++++++++++++++

PCAP
~~~~

The goal of these helpers is to make it easy to add a consistent PCAP
trace facility to an |ns3| device.  We want all of the various flavors
of PCAP tracing to work the same across all devices, so the methods of
these helpers are inherited by device helpers.  Take a look at
``src/network/helper/trace-helper.h`` if you want to follow the
discussion while looking at real code.

The class ``PcapHelperForDevice`` is a ``mixin`` provides the high
level functionality for using PCAP tracing in an |ns3| device.  Every
device must implement a single virtual method inherited from this
class.

::

  virtual void EnablePcapInternal (std::string prefix, Ptr<NetDevice> nd, bool promiscuous, bool explicitFilename) = 0;

The signature of this method reflects the device-centric view of the
situation at this level.  All of the public methods inherited from
class ``PcapUserHelperForDevice`` reduce to calling this single
device-dependent implementation method.  For example, the lowest level
PCAP method,

::

  void EnablePcap (std::string prefix, Ptr<NetDevice> nd, bool promiscuous = false, bool explicitFilename = false);

will call the device implementation of ``EnablePcapInternal``
directly.  All other public PCAP tracing methods build on this
implementation to provide additional user-level functionality.  What
this means to the user is that all device helpers in the system will
have all of the PCAP trace methods available; and these methods will
all work in the same way across devices if the device implements
``EnablePcapInternal`` correctly.

Methods
#######

::

  void EnablePcap (std::string prefix, Ptr<NetDevice> nd, bool promiscuous = false, bool explicitFilename = false);
  void EnablePcap (std::string prefix, std::string ndName, bool promiscuous = false, bool explicitFilename = false);
  void EnablePcap (std::string prefix, NetDeviceContainer d, bool promiscuous = false);
  void EnablePcap (std::string prefix, NodeContainer n, bool promiscuous = false);
  void EnablePcap (std::string prefix, uint32_t nodeid, uint32_t deviceid, bool promiscuous = false);
  void EnablePcapAll (std::string prefix, bool promiscuous = false);

In each of the methods shown above, there is a default parameter
called ``promiscuous`` that defaults to ``false``.  This parameter
indicates that the trace should not be gathered in promiscuous mode.
If you do want your traces to include all traffic seen by the device
(and if the device supports a promiscuous mode) simply add a true
parameter to any of the calls above.  For example,

::

  Ptr<NetDevice> nd;
  ...
  helper.EnablePcap ("prefix", nd, true);

will enable promiscuous mode captures on the ``NetDevice`` specified
by ``nd``.

The first two methods also include a default parameter called
``explicitFilename`` that will be discussed below.

You are encouraged to peruse the API Documentation for class
``PcapHelperForDevice`` to find the details of these methods; but to
summarize ...

* You can enable PCAP tracing on a particular node/net-device pair by
  providing a ``Ptr<NetDevice>`` to an ``EnablePcap`` method.  The
  ``Ptr<Node>`` is implicit since the net device must belong to exactly
  one Node.  For example,

  ::

    Ptr<NetDevice> nd;
    ...
    helper.EnablePcap ("prefix", nd);

* You can enable PCAP tracing on a particular node/net-device pair by
  providing a ``std::string`` representing an object name service string
  to an ``EnablePcap`` method.  The ``Ptr<NetDevice>`` is looked up from
  the name string.  Again, the ``<Node>`` is implicit since the named
  net device must belong to exactly one Node.  For example,

  ::

    Names::Add ("server" ...);
    Names::Add ("server/eth0" ...);
    ...
    helper.EnablePcap ("prefix", "server/ath0");

* You can enable PCAP tracing on a collection of node/net-device pairs
  by providing a ``NetDeviceContainer``.  For each ``NetDevice`` in the
  container the type is checked.  For each device of the proper type
  (the same type as is managed by the device helper), tracing is
  enabled.  Again, the ``<Node>`` is implicit since the found net device
  must belong to exactly one Node.  For example,

  ::

    NetDeviceContainer d = ...;
    ...
    helper.EnablePcap ("prefix", d);

* You can enable PCAP tracing on a collection of node/net-device pairs
  by providing a ``NodeContainer``.  For each Node in the
  ``NodeContainer`` its attached ``NetDevices`` are iterated.  For each
  ``NetDevice`` attached to each Node in the container, the type of that
  device is checked.  For each device of the proper type (the same type
  as is managed by the device helper), tracing is enabled.

  ::

    NodeContainer n;
    ...
    helper.EnablePcap ("prefix", n);

* You can enable PCAP tracing on the basis of Node ID and device ID as
  well as with explicit ``Ptr``.  Each Node in the system has an
  integer Node ID and each device connected to a Node has an integer
  device ID.

  ::

    helper.EnablePcap ("prefix", 21, 1);

* Finally, you can enable PCAP tracing for all devices in the system,
  with the same type as that managed by the device helper.

  ::

    helper.EnablePcapAll ("prefix");

Filenames
#########

Implicit in the method descriptions above is the construction of a
complete filename by the implementation method.  By convention, PCAP
traces in the |ns3| system are of the form ``<prefix>-<node id>-<device
id>.pcap``

As previously mentioned, every Node in the system will have a
system-assigned Node id; and every device will have an interface index
(also called a device id) relative to its node.  By default, then, a
PCAP trace file created as a result of enabling tracing on the first
device of Node 21 using the prefix "prefix" would be
``prefix-21-1.pcap``.

You can always use the |ns3| object name service to make this more
clear.  For example, if you use the object name service to assign the
name "server" to Node 21, the resulting PCAP trace file name will
automatically become, ``prefix-server-1.pcap`` and if you also assign
the name "eth0" to the device, your PCAP file name will automatically
pick this up and be called ``prefix-server-eth0.pcap``.

Finally, two of the methods shown above,

::

  void EnablePcap (std::string prefix, Ptr<NetDevice> nd, bool promiscuous = false, bool explicitFilename = false);
  void EnablePcap (std::string prefix, std::string ndName, bool promiscuous = false, bool explicitFilename = false);

have a default parameter called ``explicitFilename``.  When set to
true, this parameter disables the automatic filename completion
mechanism and allows you to create an explicit filename.  This option
is only available in the methods which enable PCAP tracing on a single
device.

For example, in order to arrange for a device helper to create a
single promiscuous PCAP capture file of a specific name
``my-pcap-file.pcap`` on a given device, one could::

  Ptr<NetDevice> nd;
  ...
  helper.EnablePcap ("my-pcap-file.pcap", nd, true, true);

The first ``true`` parameter enables promiscuous mode traces and the
second tells the helper to interpret the ``prefix`` parameter as a
complete filename.

ASCII
~~~~~

The behavior of the ASCII trace helper ``mixin`` is substantially
similar to the PCAP version.  Take a look at
``src/network/helper/trace-helper.h`` if you want to follow the
discussion while looking at real code.

The class ``AsciiTraceHelperForDevice`` adds the high level
functionality for using ASCII tracing to a device helper class.  As in
the PCAP case, every device must implement a single virtual method
inherited from the ASCII trace ``mixin``.

::

  virtual void EnableAsciiInternal (Ptr<OutputStreamWrapper> stream, 
                                    std::string prefix, 
                                    Ptr<NetDevice> nd,
                                    bool explicitFilename) = 0;


The signature of this method reflects the device-centric view of the
situation at this level; and also the fact that the helper may be
writing to a shared output stream.  All of the public
ASCII-trace-related methods inherited from class
``AsciiTraceHelperForDevice`` reduce to calling this single device-
dependent implementation method.  For example, the lowest level ascii
trace methods,

::

  void EnableAscii (std::string prefix, Ptr<NetDevice> nd, bool explicitFilename = false);
  void EnableAscii (Ptr<OutputStreamWrapper> stream, Ptr<NetDevice> nd);


will call the device implementation of ``EnableAsciiInternal``
directly, providing either a valid prefix or stream.  All other public
ASCII tracing methods will build on these low-level functions to
provide additional user-level functionality.  What this means to the
user is that all device helpers in the system will have all of the
ASCII trace methods available; and these methods will all work in the
same way across devices if the devices implement
``EnablAsciiInternal`` correctly.

Methods
#######

::

  void EnableAscii (std::string prefix, Ptr<NetDevice> nd, bool explicitFilename = false);
  void EnableAscii (Ptr<OutputStreamWrapper> stream, Ptr<NetDevice> nd);

  void EnableAscii (std::string prefix, std::string ndName, bool explicitFilename = false);
  void EnableAscii (Ptr<OutputStreamWrapper> stream, std::string ndName);

  void EnableAscii (std::string prefix, NetDeviceContainer d);
  void EnableAscii (Ptr<OutputStreamWrapper> stream, NetDeviceContainer d);

  void EnableAscii (std::string prefix, NodeContainer n);
  void EnableAscii (Ptr<OutputStreamWrapper> stream, NodeContainer n);

  void EnableAsciiAll (std::string prefix);
  void EnableAsciiAll (Ptr<OutputStreamWrapper> stream);

  void EnableAscii (std::string prefix, uint32_t nodeid, uint32_t deviceid, bool explicitFilename);
  void EnableAscii (Ptr<OutputStreamWrapper> stream, uint32_t nodeid, uint32_t deviceid);

You are encouraged to peruse the API Documentation for class
``AsciiTraceHelperForDevice`` to find the details of these methods;
but to summarize ...

* There are twice as many methods available for ASCII tracing as
  there were for PCAP tracing.  This is because, in addition to the
  PCAP-style model where traces from each unique node/device pair are
  written to a unique file, we support a model in which trace
  information for many node/device pairs is written to a common file.
  This means that the <prefix>-<node>-<device> file name generation
  mechanism is replaced by a mechanism to refer to a common file; and
  the number of API methods is doubled to allow all combinations.

* Just as in PCAP tracing, you can enable ASCII tracing on a
  particular (node, net-device) pair by providing a ``Ptr<NetDevice>``
  to an ``EnableAscii`` method.  The ``Ptr<Node>`` is implicit since
  the net device must belong to exactly one Node.  For example,

::

  Ptr<NetDevice> nd;
  ...
  helper.EnableAscii ("prefix", nd);

* The first four methods also include a default parameter called
  ``explicitFilename`` that operate similar to equivalent parameters
  in the PCAP case.

  In this case, no trace contexts are written to the ASCII trace file
  since they would be redundant.  The system will pick the file name
  to be created using the same rules as described in the PCAP section,
  except that the file will have the suffix ``.tr`` instead of
  ``.pcap``.

* If you want to enable ASCII tracing on more than one net device
  and have all traces sent to a single file, you can do that as well
  by using an object to refer to a single file.  We have already seen
  this in the "cwnd" example above::

    Ptr<NetDevice> nd1;
    Ptr<NetDevice> nd2;
    ...
    Ptr<OutputStreamWrapper> stream = asciiTraceHelper.CreateFileStream ("trace-file-name.tr");
    ...
    helper.EnableAscii (stream, nd1);
    helper.EnableAscii (stream, nd2);


  In this case, trace contexts *are* written to the ASCII trace file
  since they are required to disambiguate traces from the two devices.
  Note that since the user is completely specifying the file name, the
  string should include the ``,tr`` suffix for consistency.

* You can enable ASCII tracing on a particular (node, net-device)
  pair by providing a ``std::string`` representing an object name
  service string to an ``EnablePcap`` method.  The ``Ptr<NetDevice>``
  is looked up from the name string.  Again, the ``<Node>`` is
  implicit since the named net device must belong to exactly one Node.
  For example,

  ::

    Names::Add ("client" ...);
    Names::Add ("client/eth0" ...);
    Names::Add ("server" ...);
    Names::Add ("server/eth0" ...);
    ...
    helper.EnableAscii ("prefix", "client/eth0");
    helper.EnableAscii ("prefix", "server/eth0");

    This would result in two files named ``prefix-client-eth0.tr`` and
    ``prefix-server-eth0.tr`` with traces for each device in the
    respective trace file.  Since all of the ``EnableAscii`` functions
    are overloaded to take a stream wrapper, you can use that form as
    well::

    Names::Add ("client" ...);
    Names::Add ("client/eth0" ...);
    Names::Add ("server" ...);
    Names::Add ("server/eth0" ...);
    ...
    Ptr<OutputStreamWrapper> stream = asciiTraceHelper.CreateFileStream ("trace-file-name.tr");
    ...
    helper.EnableAscii (stream, "client/eth0");
    helper.EnableAscii (stream, "server/eth0");

  This would result in a single trace file called
  ``trace-file-name.tr`` that contains all of the trace events for
  both devices.  The events would be disambiguated by trace context
  strings.

* You can enable ASCII tracing on a collection of (node, net-device)
  pairs by providing a ``NetDeviceContainer``.  For each ``NetDevice``
  in the container the type is checked.  For each device of the proper
  type (the same type as is managed by the device helper), tracing is
  enabled.  Again, the ``<Node>`` is implicit since the found net
  device must belong to exactly one Node.  For example,

  ::

    NetDeviceContainer d = ...;
    ...
    helper.EnableAscii ("prefix", d);

    This would result in a number of ASCII trace files being created,
    each of which follows the ``<prefix>-<node id>-<device id>.tr``
    convention.

  Combining all of the traces into a single file is accomplished
  similarly to the examples above::

    NetDeviceContainer d = ...;
    ...
    Ptr<OutputStreamWrapper> stream = asciiTraceHelper.CreateFileStream ("trace-file-name.tr");
    ...
    helper.EnableAscii (stream, d);

* You can enable ASCII tracing on a collection of (node, net-device)
  pairs by providing a ``NodeContainer``.  For each Node in the
  ``NodeContainer`` its attached ``NetDevices`` are iterated.  For
  each ``NetDevice`` attached to each Node in the container, the type
  of that device is checked.  For each device of the proper type (the
  same type as is managed by the device helper), tracing is enabled.

  ::

    NodeContainer n;
    ...
    helper.EnableAscii ("prefix", n);

  This would result in a number of ASCII trace files being created,
  each of which follows the ``<prefix>-<node id>-<device id>.tr``
  convention.  Combining all of the traces into a single file is
  accomplished similarly to the examples above.

* You can enable PCAP tracing on the basis of Node ID and device ID
  as well as with explicit ``Ptr``.  Each Node in the system has an
  integer Node ID and each device connected to a Node has an integer
  device ID.

  ::

    helper.EnableAscii ("prefix", 21, 1);

  Of course, the traces can be combined into a single file as shown
  above.

* Finally, you can enable PCAP tracing for all devices in the
  system, with the same type as that managed by the device helper.

  ::

    helper.EnableAsciiAll ("prefix");

  This would result in a number of ASCII trace files being created,
  one for every device in the system of the type managed by the
  helper.  All of these files will follow the ``<prefix>-<node
  id>-<device id>.tr`` convention.  Combining all of the traces into a
  single file is accomplished similarly to the examples above.

Filenames
#########

Implicit in the prefix-style method descriptions above is the
construction of the complete filenames by the implementation method.
By convention, ASCII traces in the |ns3| system are of the form
``<prefix>-<node id>-<device id>.tr``

As previously mentioned, every Node in the system will have a
system-assigned Node id; and every device will have an interface index
(also called a device id) relative to its node.  By default, then, an
ASCII trace file created as a result of enabling tracing on the first
device of Node 21, using the prefix "prefix", would be
``prefix-21-1.tr``.

You can always use the |ns3| object name service to make this more
clear.  For example, if you use the object name service to assign the
name "server" to Node 21, the resulting ASCII trace file name will
automatically become, ``prefix-server-1.tr`` and if you also assign the
name "eth0" to the device, your ASCII trace file name will
automatically pick this up and be called ``prefix-server-eth0.tr``.

Several of the methods have a default parameter called
``explicitFilename``.  When set to true, this parameter disables the
automatic filename completion mechanism and allows you to create an
explicit filename.  This option is only available in the methods which
take a prefix and enable tracing on a single device.

Protocol Helpers
++++++++++++++++

PCAP
~~~~

The goal of these ``mixins`` is to make it easy to add a consistent
PCAP trace facility to protocols.  We want all of the various flavors
of PCAP tracing to work the same across all protocols, so the methods
of these helpers are inherited by stack helpers.  Take a look at
``src/network/helper/trace-helper.h`` if you want to follow the
discussion while looking at real code.

In this section we will be illustrating the methods as applied to the
protocol ``Ipv4``.  To specify traces in similar protocols, just
substitute the appropriate type.  For example, use a ``Ptr<Ipv6>``
instead of a ``Ptr<Ipv4>`` and call ``EnablePcapIpv6`` instead of
``EnablePcapIpv4``.

The class ``PcapHelperForIpv4`` provides the high level functionality
for using PCAP tracing in the ``Ipv4`` protocol.  Each protocol helper
enabling these methods must implement a single virtual method
inherited from this class.  There will be a separate implementation
for ``Ipv6``, for example, but the only difference will be in the
method names and signatures.  Different method names are required to
disambiguate class ``Ipv4`` from ``Ipv6`` which are both derived from
class ``Object``, and methods that share the same signature.

::

  virtual void EnablePcapIpv4Internal (std::string prefix, 
                                       Ptr<Ipv4> ipv4, 
                                       uint32_t interface,
                                       bool explicitFilename) = 0;

The signature of this method reflects the protocol and
interface-centric view of the situation at this level.  All of the
public methods inherited from class ``PcapHelperForIpv4`` reduce to
calling this single device-dependent implementation method.  For
example, the lowest level PCAP method,

::

  void EnablePcapIpv4 (std::string prefix, Ptr<Ipv4> ipv4, uint32_t interface, bool explicitFilename = false);


will call the device implementation of ``EnablePcapIpv4Internal``
directly.  All other public PCAP tracing methods build on this
implementation to provide additional user-level functionality.  What
this means to the user is that all protocol helpers in the system will
have all of the PCAP trace methods available; and these methods will
all work in the same way across protocols if the helper implements
``EnablePcapIpv4Internal`` correctly.

Methods
#######

These methods are designed to be in one-to-one correspondence with the
Node- and ``NetDevice``- centric versions of the device versions.
Instead of Node and ``NetDevice`` pair constraints, we use
protocol and interface constraints.

Note that just like in the device version, there are six methods::

  void EnablePcapIpv4 (std::string prefix, Ptr<Ipv4> ipv4, uint32_t interface, bool explicitFilename = false);
  void EnablePcapIpv4 (std::string prefix, std::string ipv4Name, uint32_t interface, bool explicitFilename = false);
  void EnablePcapIpv4 (std::string prefix, Ipv4InterfaceContainer c);
  void EnablePcapIpv4 (std::string prefix, NodeContainer n);
  void EnablePcapIpv4 (std::string prefix, uint32_t nodeid, uint32_t interface, bool explicitFilename);
  void EnablePcapIpv4All (std::string prefix);

You are encouraged to peruse the API Documentation for class
``PcapHelperForIpv4`` to find the details of these methods; but to
summarize ...

* You can enable PCAP tracing on a particular protocol/interface pair by
  providing a ``Ptr<Ipv4>`` and ``interface`` to an ``EnablePcap``
  method.  For example,

  ::

    Ptr<Ipv4> ipv4 = node->GetObject<Ipv4> ();
    ...
    helper.EnablePcapIpv4 ("prefix", ipv4, 0);

* You can enable PCAP tracing on a particular node/net-device pair by
  providing a ``std::string`` representing an object name service string
  to an ``EnablePcap`` method.  The ``Ptr<Ipv4>`` is looked up from the
  name string.  For example,

  ::

    Names::Add ("serverIPv4" ...);
    ...
    helper.EnablePcapIpv4 ("prefix", "serverIpv4", 1);

* You can enable PCAP tracing on a collection of protocol/interface
  pairs by providing an ``Ipv4InterfaceContainer``.  For each ``Ipv4`` /
  interface pair in the container the protocol type is checked.  For
  each protocol of the proper type (the same type as is managed by the
  device helper), tracing is enabled for the corresponding interface.
  For example,

  ::

    NodeContainer nodes;
    ...
    NetDeviceContainer devices = deviceHelper.Install (nodes);
    ... 
    Ipv4AddressHelper ipv4;
    ipv4.SetBase ("10.1.1.0", "255.255.255.0");
    Ipv4InterfaceContainer interfaces = ipv4.Assign (devices);
    ...
    helper.EnablePcapIpv4 ("prefix", interfaces);

* You can enable PCAP tracing on a collection of protocol/interface
  pairs by providing a ``NodeContainer``.  For each Node in the
  ``NodeContainer`` the appropriate protocol is found.  For each
  protocol, its interfaces are enumerated and tracing is enabled on the
  resulting pairs.  For example,

  ::

    NodeContainer n;
    ...
    helper.EnablePcapIpv4 ("prefix", n);

* You can enable PCAP tracing on the basis of Node ID and interface as
  well.  In this case, the node-id is translated to a ``Ptr<Node>`` and
  the appropriate protocol is looked up in the node.  The resulting
  protocol and interface are used to specify the resulting trace source.

  ::

    helper.EnablePcapIpv4 ("prefix", 21, 1);

* Finally, you can enable PCAP tracing for all interfaces in the
  system, with associated protocol being the same type as that managed
  by the device helper.

  ::

    helper.EnablePcapIpv4All ("prefix");

Filenames
#########

Implicit in all of the method descriptions above is the construction
of the complete filenames by the implementation method.  By
convention, PCAP traces taken for devices in the |ns3| system are of
the form "<prefix>-<node id>-<device id>.pcap".  In the case of
protocol traces, there is a one-to-one correspondence between
protocols and ``Nodes``.  This is because protocol ``Objects`` are
aggregated to ``Node Objects``.  Since there is no global protocol id
in the system, we use the corresponding Node id in file naming.
Therefore there is a possibility for file name collisions in
automatically chosen trace file names.  For this reason, the file name
convention is changed for protocol traces.

As previously mentioned, every Node in the system will have a
system-assigned Node id.  Since there is a one-to-one correspondence
between protocol instances and Node instances we use the Node id.
Each interface has an interface id relative to its protocol.  We use
the convention "<prefix>-n<node id>-i<interface id>.pcap" for trace
file naming in protocol helpers.

Therefore, by default, a PCAP trace file created as a result of
enabling tracing on interface 1 of the Ipv4 protocol of Node 21 using
the prefix "prefix" would be "prefix-n21-i1.pcap".

You can always use the |ns3| object name service to make this more
clear.  For example, if you use the object name service to assign the
name "serverIpv4" to the Ptr<Ipv4> on Node 21, the resulting PCAP
trace file name will automatically become,
"prefix-nserverIpv4-i1.pcap".

Several of the methods have a default parameter called
``explicitFilename``.  When set to true, this parameter disables the
automatic filename completion mechanism and allows you to create an
explicit filename.  This option is only available in the methods which
take a prefix and enable tracing on a single device.

ASCII
~~~~~

The behavior of the ASCII trace helpers is substantially similar to
the PCAP case.  Take a look at ``src/network/helper/trace-helper.h``
if you want to follow the discussion while looking at real code.

In this section we will be illustrating the methods as applied to the
protocol ``Ipv4``.  To specify traces in similar protocols, just
substitute the appropriate type.  For example, use a ``Ptr<Ipv6>``
instead of a ``Ptr<Ipv4>`` and call ``EnableAsciiIpv6`` instead of
``EnableAsciiIpv4``.

The class ``AsciiTraceHelperForIpv4`` adds the high level
functionality for using ASCII tracing to a protocol helper.  Each
protocol that enables these methods must implement a single virtual
method inherited from this class.

::

  virtual void EnableAsciiIpv4Internal (Ptr<OutputStreamWrapper> stream, 
                                        std::string prefix, 
                                        Ptr<Ipv4> ipv4, 
                                        uint32_t interface,
                                        bool explicitFilename) = 0;

The signature of this method reflects the protocol- and
interface-centric view of the situation at this level; and also the
fact that the helper may be writing to a shared output stream.  All of
the public methods inherited from class
``PcapAndAsciiTraceHelperForIpv4`` reduce to calling this single
device- dependent implementation method.  For example, the lowest
level ASCII trace methods,

::

  void EnableAsciiIpv4 (std::string prefix, Ptr<Ipv4> ipv4, uint32_t interface, bool explicitFilename = false);
  void EnableAsciiIpv4 (Ptr<OutputStreamWrapper> stream, Ptr<Ipv4> ipv4, uint32_t interface);


will call the device implementation of ``EnableAsciiIpv4Internal``
directly, providing either the prefix or the stream.  All other public
ASCII tracing methods will build on these low-level functions to
provide additional user-level functionality.  What this means to the
user is that all device helpers in the system will have all of the
ASCII trace methods available; and these methods will all work in the
same way across protocols if the protocols implement
``EnablAsciiIpv4Internal`` correctly.

Methods
#######

::

  void EnableAsciiIpv4 (std::string prefix, Ptr<Ipv4> ipv4, uint32_t interface, bool explicitFilename = false);
  void EnableAsciiIpv4 (Ptr<OutputStreamWrapper> stream, Ptr<Ipv4> ipv4, uint32_t interface);

  void EnableAsciiIpv4 (std::string prefix, std::string ipv4Name, uint32_t interface, bool explicitFilename = false);
  void EnableAsciiIpv4 (Ptr<OutputStreamWrapper> stream, std::string ipv4Name, uint32_t interface);

  void EnableAsciiIpv4 (std::string prefix, Ipv4InterfaceContainer c);
  void EnableAsciiIpv4 (Ptr<OutputStreamWrapper> stream, Ipv4InterfaceContainer c);

  void EnableAsciiIpv4 (std::string prefix, NodeContainer n);
  void EnableAsciiIpv4 (Ptr<OutputStreamWrapper> stream, NodeContainer n);

  void EnableAsciiIpv4All (std::string prefix);
  void EnableAsciiIpv4All (Ptr<OutputStreamWrapper> stream);

  void EnableAsciiIpv4 (std::string prefix, uint32_t nodeid, uint32_t deviceid, bool explicitFilename);
  void EnableAsciiIpv4 (Ptr<OutputStreamWrapper> stream, uint32_t nodeid, uint32_t interface);

You are encouraged to peruse the API Documentation for class
``PcapAndAsciiHelperForIpv4`` to find the details of these methods;
but to summarize ...

* There are twice as many methods available for ASCII tracing as there
  were for PCAP tracing.  This is because, in addition to the PCAP-style
  model where traces from each unique protocol/interface pair are
  written to a unique file, we support a model in which trace
  information for many protocol/interface pairs is written to a common
  file.  This means that the <prefix>-n<node id>-<interface> file name
  generation mechanism is replaced by a mechanism to refer to a common
  file; and the number of API methods is doubled to allow all
  combinations.

* Just as in PCAP tracing, you can enable ASCII tracing on a particular
  protocol/interface pair by providing a ``Ptr<Ipv4>`` and an
  ``interface`` to an ``EnableAscii`` method.  For example,

  ::

    Ptr<Ipv4> ipv4;
    ...
    helper.EnableAsciiIpv4 ("prefix", ipv4, 1);

  In this case, no trace contexts are written to the ASCII trace file
  since they would be redundant.  The system will pick the file name to
  be created using the same rules as described in the PCAP section,
  except that the file will have the suffix ".tr" instead of ".pcap".

* If you want to enable ASCII tracing on more than one interface and
  have all traces sent to a single file, you can do that as well by
  using an object to refer to a single file.  We have already something
  similar to this in the "cwnd" example above::

    Ptr<Ipv4> protocol1 = node1->GetObject<Ipv4> ();
    Ptr<Ipv4> protocol2 = node2->GetObject<Ipv4> ();
    ...
    Ptr<OutputStreamWrapper> stream = asciiTraceHelper.CreateFileStream ("trace-file-name.tr");
    ...
    helper.EnableAsciiIpv4 (stream, protocol1, 1);
    helper.EnableAsciiIpv4 (stream, protocol2, 1);

  In this case, trace contexts are written to the ASCII trace file since
  they are required to disambiguate traces from the two interfaces.
  Note that since the user is completely specifying the file name, the
  string should include the ",tr" for consistency.

* You can enable ASCII tracing on a particular protocol by providing a
  ``std::string`` representing an object name service string to an
  ``EnablePcap`` method.  The ``Ptr<Ipv4>`` is looked up from the name
  string.  The ``<Node>`` in the resulting filenames is implicit since
  there is a one-to-one correspondence between protocol instances and
  nodes, For example,

  ::

    Names::Add ("node1Ipv4" ...);
    Names::Add ("node2Ipv4" ...);
    ...
    helper.EnableAsciiIpv4 ("prefix", "node1Ipv4", 1);
    helper.EnableAsciiIpv4 ("prefix", "node2Ipv4", 1);

  This would result in two files named "prefix-nnode1Ipv4-i1.tr" and
  "prefix-nnode2Ipv4-i1.tr" with traces for each interface in the
  respective trace file.  Since all of the EnableAscii functions are
  overloaded to take a stream wrapper, you can use that form as well::

    Names::Add ("node1Ipv4" ...);
    Names::Add ("node2Ipv4" ...);
    ...
    Ptr<OutputStreamWrapper> stream = asciiTraceHelper.CreateFileStream ("trace-file-name.tr");
    ...
    helper.EnableAsciiIpv4 (stream, "node1Ipv4", 1);
    helper.EnableAsciiIpv4 (stream, "node2Ipv4", 1);

  This would result in a single trace file called "trace-file-name.tr"
  that contains all of the trace events for both interfaces.  The events
  would be disambiguated by trace context strings.

* You can enable ASCII tracing on a collection of protocol/interface
  pairs by providing an ``Ipv4InterfaceContainer``.  For each protocol
  of the proper type (the same type as is managed by the device helper),
  tracing is enabled for the corresponding interface.  Again, the
  ``<Node>`` is implicit since there is a one-to-one correspondence
  between each protocol and its node.  For example,

  ::

    NodeContainer nodes;
    ...
    NetDeviceContainer devices = deviceHelper.Install (nodes);
    ... 
    Ipv4AddressHelper ipv4;
    ipv4.SetBase ("10.1.1.0", "255.255.255.0");
    Ipv4InterfaceContainer interfaces = ipv4.Assign (devices);
    ...
    ...
    helper.EnableAsciiIpv4 ("prefix", interfaces);

  This would result in a number of ASCII trace files being created, each
  of which follows the <prefix>-n<node id>-i<interface>.tr convention.
  Combining all of the traces into a single file is accomplished
  similarly to the examples above::

    NodeContainer nodes;
    ...
    NetDeviceContainer devices = deviceHelper.Install (nodes);
    ... 
    Ipv4AddressHelper ipv4;
    ipv4.SetBase ("10.1.1.0", "255.255.255.0");
    Ipv4InterfaceContainer interfaces = ipv4.Assign (devices);
    ...
    Ptr<OutputStreamWrapper> stream = asciiTraceHelper.CreateFileStream ("trace-file-name.tr");
    ...
    helper.EnableAsciiIpv4 (stream, interfaces);

* You can enable ASCII tracing on a collection of protocol/interface
  pairs by providing a ``NodeContainer``.  For each Node in the
  ``NodeContainer`` the appropriate protocol is found.  For each
  protocol, its interfaces are enumerated and tracing is enabled on the
  resulting pairs.  For example,

  ::

    NodeContainer n;
    ...
    helper.EnableAsciiIpv4 ("prefix", n);

  This would result in a number of ASCII trace files being created,
  each of which follows the <prefix>-<node id>-<device id>.tr
  convention.  Combining all of the traces into a single file is
  accomplished similarly to the examples above.

* You can enable PCAP tracing on the basis of Node ID and device ID as
  well.  In this case, the node-id is translated to a ``Ptr<Node>`` and
  the appropriate protocol is looked up in the node.  The resulting
  protocol and interface are used to specify the resulting trace source.

  ::

    helper.EnableAsciiIpv4 ("prefix", 21, 1);

  Of course, the traces can be combined into a single file as shown
  above.

* Finally, you can enable ASCII tracing for all interfaces in the
  system, with associated protocol being the same type as that managed
  by the device helper.

  ::

    helper.EnableAsciiIpv4All ("prefix");

  This would result in a number of ASCII trace files being created, one
  for every interface in the system related to a protocol of the type
  managed by the helper.  All of these files will follow the
  <prefix>-n<node id>-i<interface.tr convention.  Combining all of the
  traces into a single file is accomplished similarly to the examples
  above.

Filenames
#########

Implicit in the prefix-style method descriptions above is the
construction of the complete filenames by the implementation method.
By convention, ASCII traces in the |ns3| system are of the form
"<prefix>-<node id>-<device id>.tr"

As previously mentioned, every Node in the system will have a
system-assigned Node id.  Since there is a one-to-one correspondence
between protocols and nodes we use to node-id to identify the protocol
identity.  Every interface on a given protocol will have an interface
index (also called simply an interface) relative to its protocol.  By
default, then, an ASCII trace file created as a result of enabling
tracing on the first device of Node 21, using the prefix "prefix",
would be "prefix-n21-i1.tr".  Use the prefix to disambiguate multiple
protocols per node.

You can always use the |ns3| object name service to make this more
clear.  For example, if you use the object name service to assign the
name "serverIpv4" to the protocol on Node 21, and also specify
interface one, the resulting ASCII trace file name will automatically
become, "prefix-nserverIpv4-1.tr".

Several of the methods have a default parameter called
``explicitFilename``.  When set to true, this parameter disables the
automatic filename completion mechanism and allows you to create an
explicit filename.  This option is only available in the methods which
take a prefix and enable tracing on a single device.

Summary
*******

|ns3| includes an extremely rich environment allowing users at several
levels to customize the kinds of information that can be extracted
from simulations.

There are high-level helper functions that allow users to simply
control the collection of pre-defined outputs to a fine granularity.
There are mid-level helper functions to allow more sophisticated users
to customize how information is extracted and saved; and there are
low-level core functions to allow expert users to alter the system to
present new and previously unexported information in a way that will
be immediately accessible to users at higher levels.

This is a very comprehensive system, and we realize that it is a lot
to digest, especially for new users or those not intimately familiar
with C++ and its idioms.  We do consider the tracing system a very
important part of |ns3| and so recommend becoming as familiar as
possible with it.  It is probably the case that understanding the rest
of the |ns3| system will be quite simple once you have mastered the
tracing system
