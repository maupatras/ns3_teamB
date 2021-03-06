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
	> Current file was initially translated by Costas Deltouzos.
	> Last update was performed at 2015-04-27 by Costas Deltouzos.
	========================================================================================

Μικρορυθμίσεις
--------------

.. _UsingLogging:

Χρησιμοποιώντας την Ενότητα Καταγραφής
**************************************

..
	We have already taken a brief look at the |ns3| logging module while
  going over the ``first.cc`` script.  We will now take a closer look and 
  see what kind of use-cases the logging subsystem was designed to cover.

Έχουμε ήδη δει σύντομα την ενότητα καταγραφής του |ns3| κατά τη διάρκεια 
του σεναρίου προσομοίωσης ``first.cc``. Εδώ θα εξετάσουμε εις βάθος το σύστημα 
καταγραφής και τις περιπτώσεις χρήσης τις οποίες παρέχει.


Σύνοψη Καταγραφής
+++++++++++++++++
..
  Many large systems support some kind of message logging facility, and 
  |ns3| is not an exception.  In some cases, only error messages are 
  logged to the "operator console" (which is typically ``stderr`` in Unix-
  based systems).  In other systems, warning messages may be output as well as 
  more detailed informational messages.  In some cases, logging facilities are 
  used to output debug messages which can quickly turn the output into a blur.

Είναι κοινή πρακτική τα μεγάλα συστήματα, όπως το |ns3|, να παρέχουν τη δυνατότητα 
καταγραφής μηνυμάτων. Σε μερικές περιπτώσεις καταγράφονται μόνο τα μηνύματα λάθους 
στο "operator console" (που τυπικά είναι το ``stderr`` στα συστήματα Unix). Σε άλλα 
συστήματα, καταγράφονται μηνύματα προειδοποίησης καθώς και άλλα μηνύματα που 
παρέχουν περισσότερη πληροφορία. Τέλος σε μερικές περιπτώσεις, η διαδικασία καταγραφής
χρησιμοποιείται για να παράγει μηνύματα αποσφαλμάτωσης που όταν παρουσιαστούν θολώνουν
την έξοδο.

..
  |ns3| takes the view that all of these verbosity levels are useful 
  and we provide a selectable, multi-level approach to message logging.  Logging
  can be disabled completely, enabled on a component-by-component basis, or
  enabled globally; and it provides selectable verbosity levels.  The 
  |ns3| log module provides a straightforward, relatively easy to use
  way to get useful information out of your simulation.

Κατά το |ns3|, όλα τα παραπάνω επίπεδα καταγραφής είναι χρήσιμα και για το λόγο αυτό
παρέχεται η δυνατότητα καταγραφής μηνυμάτων σε πολλαπλά επίπεδα. Η διαδικασία 
καταγραφής μπορεί να απενεργοποιηθεί πλήρως, να ενεργοποιηθεί ανά συστατικό 
στοιχείο (component), ή να ενεργοποιηθεί συνολικά στο σύστημα. Επίσης δίνεται η 
δυνατότητα επιλογής του εύρους των μηνυμάτων σε επίπεδα. Η ενότητα καταγραφής του
|ns3| παρέχει έναν απλό και εύκολο τρόπο για εξαγωγή χρήσιμων πληροφοριών από μια
προσομοίωση.

..
  You should understand that we do provide a general purpose mechanism --- 
  tracing --- to get data out of your models which should be preferred for 
  simulation output (see the tutorial section Using the Tracing System for
  more details on our tracing system).  Logging should be preferred for 
  debugging information, warnings, error messages, or any time you want to 
  easily get a quick message out of your scripts or models.

Πρέπει να γίνει σαφές ότι παρέχεται ένας μηχανισμός γενικού σκοπού --- ιχνηλασίας
--- για την εξαγωγή δεδομένων από τα μοντέλα του χρήστη, ο οποίος θα έπρεπε να 
προτιμάται για την έξοδο της εξομοίωσης (δείτε τον τομέα "Χρησιμοποιώντας το 
Σύστημα Ιχνηλασίας" για περισσότερες πληροφορίες). Ο μηχανισμός της καταγραφής 
αντίστοιχα θα πρέπει να προτιμάται για πληροφορίες αποσφαλμάτωσης, για μηνύματα 
προειδοποίησης και λάθους, καθώς και για περιπτώσεις που χρειάζεται να πάρουμε 
ένα γρήγορο μήνυμα από ένα μοντέλο ή σενάριο προσομοίωσης.

..
  There are currently seven levels of log messages of increasing verbosity
  defined in the system.  

Στο σύστημα έχουν οριστεί προς το παρόν επτά επίπεδα μηνυμάτων καταγραφής 
αυξανόμενου εύρους μηνυμάτων.

..
  * LOG_ERROR --- Log error messages (associated macro: NS_LOG_ERROR);
  * LOG_WARN --- Log warning messages (associated macro: NS_LOG_WARN);
  * LOG_DEBUG --- Log relatively rare, ad-hoc debugging messages
    (associated macro: NS_LOG_DEBUG);
  * LOG_INFO --- Log informational messages about program progress
    (associated macro: NS_LOG_INFO);
  * LOG_FUNCTION --- Log a message describing each function called
    (two associated macros: NS_LOG_FUNCTION, used for member functions,
    and NS_LOG_FUNCTION_NOARGS, used for static functions);
  * LOG_LOGIC -- Log messages describing logical flow within a function
    (associated macro: NS_LOG_LOGIC);
  * LOG_ALL --- Log everything mentioned above (no associated macro).

* LOG_ERROR --- Καταγραφή μηνυμάτων λάθους (σχετική μακροεντολή: NS_LOG_ERROR);
* LOG_WARN --- Καταγραφή μηνυμάτων προειδοποίησης (σχετική μακροεντολή: NS_LOG_WARN);
* LOG_DEBUG --- Καταγραφή σχετικά σπανίων, προσαρμοσμένων μηνυμάτων αποσφαλμάτωσης (σχετική μακροεντολή: NS_LOG_DEBUG);
* LOG_INFO --- Καταγραφή πληροφοριακών μηνυμάτων για την πρόοδο του προγράμματος (σχετική μακροεντολή: NS_LOG_INFO);
* LOG_FUNCTION --- Καταγραφή ενός μηνύματος περιγραφής για κάθε συνάρτηση που καλείται (δύο σχετικές μακροεντολές: NS_LOG_FUNCTION, που χρησιμοποιείται για member functions, και NS_LOG_FUNCTION_NOARGS, που χρησιμοποιείται για static functions);
* LOG_LOGIC -- Καταγραφή μηνυμάτων που περιγράφουν τη λογική ροή μέσα σε μια συνάρτηση (σχετική μακροεντολή: NS_LOG_LOGIC);
* LOG_ALL --- Καταγραφή όλων των παραπάνω (καμία σχετική μακροεντολή).

..
  For each LOG_TYPE there is also LOG_LEVEL_TYPE that, if used, enables
  logging of all the levels above it in addition to it's level.  (As a
  consequence of this, LOG_ERROR and LOG_LEVEL_ERROR and also LOG_ALL
  and LOG_LEVEL_ALL are functionally equivalent.)  For example,
  enabling LOG_INFO will only enable messages provided by NS_LOG_INFO macro,
  while enabling LOG_LEVEL_INFO will also enable messages provided by
  NS_LOG_DEBUG, NS_LOG_WARN and NS_LOG_ERROR macros.  

Για κάθε επίπεδο, εκτός από το LOG_TYPE υπάρχει και το LOG_LEVEL_TYPE το οποίο
αν χρησιμοποιηθεί, ενεργοποιεί την καταγραφή όλων των επιπέδων που βρίσκονται 
πάνω από αυτό. (Κατά συνέπεια, τα LOG_ERROR και LOG_LEVEL_ERROR είναι ισοδύναμα
μεταξύ τους όπως επίσης και τα LOG_ALL και and LOG_LEVEL_ALL.) Για παράδειγμα, 
ενεργοποιώντας το LOG_INFO θα παραχθούν μόνο μηνύματα από την μακροεντολή 
NS_LOG_INFO, ενώ ενεργοποιώντας το LOG_LEVEL_INFO θα παραχθούν επίσης και μηνύματα
από τις μακροεντολές NS_LOG_DEBUG, NS_LOG_WARN και NS_LOG_ERROR.

..
	We also provide an unconditional logging macro that is always displayed,
	irrespective of logging levels or component selection.

Επίσης παρέχεται μια μακροεντολή καταγραφής χωρίς περιορισμούς, ανεξάρτητη από τα 
επίπεδα καταγραφής ή την επιλογή συστατικού στοιχείου.

..
	* NS_LOG_UNCOND -- Log the associated message unconditionally (no associated
	  log level).
  
* NS_LOG_UNCOND -- Καταγραφή του μηνύματος χωρίς περιορισμούς (κανένα σχετικό επίπεδο καταγραφής).

..
	  Each level can be requested singly or cumulatively; and logging can be set 
	  up using a shell environment variable (NS_LOG) or by logging system function 
	  call.  As was seen earlier in the tutorial, the logging system has Doxygen 
	  documentation and now would be a good time to peruse the Logging Module 
	  documentation if you have not done so.

Το κάθε επίπεδο μπορεί να ζητηθεί μεμονωμένο ή σε συνδυασμό με τα άλλα. Η καταγραφή
αντίστοιχα μπορεί να ρυθμιστεί μέσω μιας μεταβλητής του περιβάλλοντος φλοιού 
(NS_LOG) ή μέσω καταγραφής της κλήση συνάρτησης συστήματος. Όπως έχουμε ήδη δει 
νωρίτερα σε αυτό τον οδηγό, το σύστημα καταγραφής έχει τεκμηρίωση Doxygen και ίσως
είναι χρήσιμο να μελετήσετε την τεκμηρίωση της Ενότητας Καταγραφής.

..
	Now that you have read the documentation in great detail, let's use some of
	that knowledge to get some interesting information out of the 
	``scratch/myfirst.cc`` example script you have already built.

Αφού έχετε διαβάσει την τεκμηρίωση σε μεγάλο βαθμό, είναι ώρα να χρησιμοποιήσουμε
αυτή τη γνώση για να εξάγουμε μερικές χρήσιμες πληροφορίες από το παράδειγμα 
``scratch/myfirst.cc`` που έχετε ήδη δημιουργήσει.

..
	Enabling Logging
	++++++++++++++++

	Let's use the NS_LOG environment variable to turn on some more logging, but
	first, just to get our bearings, go ahead and run the last script just as you 
	did previously,

Ενεργοποίηση Καταγραφής
+++++++++++++++++++++++

Μπορούμε να χρησιμοποιήσουμε τη μεταβλητή συστήματος NS_LOG για να ενεργοποιήσουμε 
επιπλέον λειτουργίες καταγραφής, αλλά ας τρέξουμε αρχικά το παρακάτω σενάριο

.. sourcecode:: bash

  $ ./waf --run scratch/myfirst

..
	You should see the now familiar output of the first |ns3| example
	program

Θα πρέπει να βλέπετε τώρα τη γνώριμη έξοδο του πρώτου παραδείγματος από τα
προγράμματα του |ns3|.

.. sourcecode:: bash

  $ Waf: Entering directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  Waf: Leaving directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  'build' finished successfully (0.413s)
  Sent 1024 bytes to 10.1.1.2
  Received 1024 bytes from 10.1.1.1
  Received 1024 bytes from 10.1.1.2

..
	It turns out that the "Sent" and "Received" messages you see above are
	actually logging messages from the ``UdpEchoClientApplication`` and 
	``UdpEchoServerApplication``.  We can ask the client application, for 
	example, to print more information by setting its logging level via the 
	NS_LOG environment variable.  

Τα μηνύματα "Sent" και "Received" που βλέπετε παραπάνω είναι στην πραγματικότητα
μηνύματα καταγραφής από τα ``UdpEchoClientApplication`` και 
``UdpEchoServerApplication``. Μπορούμε, για παράδειγμα, να ζητήσουμε από την 
εφαρμογή του χρήστη να τυπώσει περισσότερες πληροφορίες θέτοντας το επίπεδο 
καταγραφής της μέσω της μεταβλητής περιβάλλοντος NS_LOG.

..
	I am going to assume from here on that you are using an sh-like shell that uses 
	the"VARIABLE=value" syntax.  If you are using a csh-like shell, then you 
	will have to convert my examples to the "setenv VARIABLE value" syntax 
	required by those shells.

Στη συνέχεια θεωρούμε ότι ο χρήστης χρησιμοποιεί έναν φλοιό που χρησιμοποιεί τη 
σύνταξη "VARIABLE=value" όπως ο sh. Αν χρησιμοποιείτε ένα φλοιό τύπου csh, τότε πρέπει
να μετασχηματίσετε τα παρακάτω παραδείγματα  σε σύνταξη "setenv VARIABLE value".

..
	Right now, the UDP echo client application is responding to the following line
	of code in ``scratch/myfirst.cc``,

Αυτή τη στιγμή, η εφαρμογή UDP echo client ανταποκρίνεται στην παρακάτω γραμμή
κώδικα του ``scratch/myfirst.cc``,

::

  LogComponentEnable("UdpEchoClientApplication", LOG_LEVEL_INFO);

..
	This line of code enables the ``LOG_LEVEL_INFO`` level of logging.  When 
	we pass a logging level flag, we are actually enabling the given level and
	all lower levels.  In this case, we have enabled ``NS_LOG_INFO``,
	``NS_LOG_DEBUG``, ``NS_LOG_WARN`` and ``NS_LOG_ERROR``.  We can
	increase the logging level and get more information without changing the
	script and recompiling by setting the NS_LOG environment variable like this:

Αυτή η γραμμή κώδικα ενεργοποιεί το επίπεδο καταγραφής ``LOG_LEVEL_INFO``. Όταν
περάσουμε κάποια παράμετρο επιπέδου καταγραφής, στην ουσία ενεργοποιούμε το 
συγκεκριμένο επίπεδο και όλα τα χαμηλότερά του. Στο συγκεκριμένο παράδειγμα, ενεργοποιούμε
τα ``NS_LOG_INFO``, ``NS_LOG_DEBUG``, ``NS_LOG_WARN`` και ``NS_LOG_ERROR``. Μπορούμε
να αυξήσουμε το επίπεδο καταγραφής και να πάρουμε περισσότερες πληροφορίες χωρίς να 
χρειαστεί να αλλάξουμε το σενάριο και να επαναμεταγλωτίσσουμε, αν θέσουμε τη μεταβλητή
περιβάλλοντος NS_LOG ως εξής:

.. sourcecode:: bash

  $ export NS_LOG=UdpEchoClientApplication=level_all

..
	This sets the shell environment variable ``NS_LOG`` to the string,

Αυτό θα θέσει τη μεταβλητή περιβάλλοντος ``NS_LOG`` στο αλφαριθμητικό,

.. sourcecode:: bash

  UdpEchoClientApplication=level_all

..
	The left hand side of the assignment is the name of the logging component we
	want to set, and the right hand side is the flag we want to use.  In this case,
	we are going to turn on all of the debugging levels for the application.  If
	you run the script with NS_LOG set this way, the |ns3| logging 
	system will pick up the change and you should see the following output:

Το αριστερό σκέλος της ανάθεσης είναι το όνομα του στοιχείου καταγραφής που θέλουμε να
θέσουμε, ενώ το δεξί σκέλος είναι το όρισμα που θέλουμε να χρησιμοποιήσουμε. Στην
περίπτωσή μας θα ενεργοποιήσουμε όλα τα επίπεδα αποσφαλμάτωσης για τη συγκεκριμένη
εφαρμογή. Αν τρέξετε το σενάριο θέτοντας την NS_LOG με αυτόν τον τρόπο, το σύστημα
καταγραφής του |ns3| θα δει την αλλαγή και θα πρέπει να δείτε την παρακάτω έξοδο:

.. sourcecode:: bash

  Waf: Entering directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  Waf: Leaving directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  'build' finished successfully (0.404s)
  UdpEchoClientApplication:UdpEchoClient()
  UdpEchoClientApplication:SetDataSize(1024)
  UdpEchoClientApplication:StartApplication()
  UdpEchoClientApplication:ScheduleTransmit()
  UdpEchoClientApplication:Send()
  Sent 1024 bytes to 10.1.1.2
  Received 1024 bytes from 10.1.1.1
  UdpEchoClientApplication:HandleRead(0x6241e0, 0x624a20)
  Received 1024 bytes from 10.1.1.2
  UdpEchoClientApplication:StopApplication()
  UdpEchoClientApplication:DoDispose()
  UdpEchoClientApplication:~UdpEchoClient()

..
	The additional debug information provided by the application is from
	the NS_LOG_FUNCTION level.  This shows every time a function in the application
	is called during script execution.  Generally, use of (at least)
	NS_LOG_FUNCTION(this) in member functions is preferred. Use
	NS_LOG_FUNCTION_NOARGS() only in static functions.  Note, however, that there
	are no requirements in the |ns3| system that models must support any particular
	logging  functionality.  The decision regarding how much information is logged
	is left to the individual model developer.  In the case of the echo
	applications, a good deal of log output is available.

Η πρόσθετη πληροφορία αποσφαλμάτωσης που παρέχεται από την εφαρμογή 
βρίσκεται στο επίπεδο NS_LOG_FUNCTION. Αυτή εμφανίζεται κάθε φορά που καλείται
μια συνάρτηση της εφαρμογής. Γενικά η χρήση της NS_LOG_FUNCTION(this)
ενδείκνυται σε member functions, ενώ η NS_LOG_FUNCTION_NOARGS() σε static 
functions. Σημειώστε όμως ότι στο σύστημα |ns3|, δεν υπάρχει η απαίτηση τα
μοντέλα να υποστηρίζουν κάποια συγκεκριμένη λειτουργία καταγραφής. Η 
απόφαση για το εύρος της πληροφορίας που καταγράφεται, επαφίεται στον 
προγραμματιστή του μοντέλου. Σε περίπτωση εφαρμογής αντανάκλασης, ένα μεγάλο
μέρος της εξόδου καταγραφής είναι διαθέσιμο.

..
	You can now see a log of the function calls that were made to the application.
	If you look closely you will notice a single colon between the string 
	``UdpEchoClientApplication`` and the method name where you might have 
	expected a C++ scope operator (``::``).  This is intentional.  


Μπορείτε να δείτε μια καταγραφή των κλήσεων σε συναρτήσεις που έγιναν
στην εφαρμογή. Αν κοιτάξετε προσεκτικά, θα παρατηρήσετε μια μονή στήλη μεταξύ του
αλφαριθμητικού ``UdpEchoClientApplication`` και του ονόματος της μεθόδου, αντί 
του τελεστή ``::`` της C++ που θα περιμένατε. Αυτό γίνεται εσκεμμένα.

..
	The name is not actually a class name, it is a logging component name.  When 
	there is a one-to-one correspondence between a source file and a class, this 
	will generally be the class name but you should understand that it is not 
	actually a class name, and there is a single colon there instead of a double
	colon to remind you in a relatively subtle way to conceptually separate the 
	logging component name from the class name.

Το όνομα δεν είναι στην πραγματικότητα το όνομα μιας κλάσης, αλλά το όνομα
του στοιχείου καταγραφής. Όταν υπάρχει αντιστοίχηση 1-προς-1 μεταξύ του 
αρχείου πηγής και της κλάσης, το όνομα θα είναι γενικά ίδιο με της κλάσης. Η 
μονή στήλη χρησιμοποιείται αντί της διπλής για να διαχωρίσει το στοιχείο 
καταγραφής από το όνομα της κλάσης.

..
	It turns out that in some cases, it can be hard to determine which method
	actually generates a log message.  If you look in the text above, you may
	wonder where the string "``Received 1024 bytes from 10.1.1.2``" comes
	from.  You can resolve this by OR'ing the ``prefix_func`` level into the
	``NS_LOG`` environment variable.  Try doing the following,

Σε μερικές περιπτώσεις είναι δύσκολο να προσδιορίσεις ποια μέθοδος ακριβώς
παράγει ένα μήνυμα καταγραφής. Αν κοιτάξετε το παραπάνω κείμενο, ίσως 
αναρωτιέστε από που προέρχεται το αλφαριθμητικό "``Received 1024 bytes from 
10.1.1.2``". Μπορείτε να λύσετε την απορία σας μέσω του επιπέδου ``prefix_func``
στη μεταβλητή συστήματος ``NS_LOG``. Δοκιμάστε το παρακάτω,

.. sourcecode:: bash

  $ export 'NS_LOG=UdpEchoClientApplication=level_all|prefix_func'

..
	Note that the quotes are required since the vertical bar we use to indicate an
	OR operation is also a Unix pipe connector.

Σημειώστε ότι τα εισαγωγικά χρειάζονται, αφού η κάθετη στήλη χρησιμοποιείται 
στο Unix για να υποδείξει τον τελεστή Ή.

..
	Now, if you run the script you will see that the logging system makes sure 
	that every message from the given log component is prefixed with the component
	name.

Τώρα αν τρέξετε το σενάριο, θα παρατηρήσετε ότι το σύστημα καταγραφής προσθέτει
σε κάθε μήνυμα ένα πρόθεμα με το όνομα του στοιχείου.

.. sourcecode:: bash

  Waf: Entering directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  Waf: Leaving directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  'build' finished successfully (0.417s)
  UdpEchoClientApplication:UdpEchoClient()
  UdpEchoClientApplication:SetDataSize(1024)
  UdpEchoClientApplication:StartApplication()
  UdpEchoClientApplication:ScheduleTransmit()
  UdpEchoClientApplication:Send()
  UdpEchoClientApplication:Send(): Sent 1024 bytes to 10.1.1.2
  Received 1024 bytes from 10.1.1.1
  UdpEchoClientApplication:HandleRead(0x6241e0, 0x624a20)
  UdpEchoClientApplication:HandleRead(): Received 1024 bytes from 10.1.1.2
  UdpEchoClientApplication:StopApplication()
  UdpEchoClientApplication:DoDispose()
  UdpEchoClientApplication:~UdpEchoClient()

..
	You can now see all of the messages coming from the UDP echo client application
	are identified as such.  The message "Received 1024 bytes from 10.1.1.2" is
	now clearly identified as coming from the echo client application.  The 
	remaining message must be coming from the UDP echo server application.  We 
	can enable that component by entering a colon separated list of components in
	the NS_LOG environment variable.

Μπορείτε να ταυτοποιήσετε τώρα όλα τα μηνύματα που προέρχονται από την εφαρμογή 
πελάτη UDP echo. Το μήνυμα "Received 1024 bytes from 10.1.1.2" φαίνεται τώρα ξεκάθαρα 
ότι προέρχεται από την εφαρμογή του πελάτη. Αντίστοιχα το άλλο μήνυμα προέρχεται από 
την εφαρμογή του εξυπηρετητή UDP echo. Μπορούμε να ενεργοποιήσουμε το στοιχείο αυτό, 
προσθέτοντας μια λίστα στοιχείων στη μεταβλητή περιβάλλοντος NS_LOG.
 
.. sourcecode:: bash

  $ export 'NS_LOG=UdpEchoClientApplication=level_all|prefix_func:
                 UdpEchoServerApplication=level_all|prefix_func'

..
	Warning:  You will need to remove the newline after the ``:`` in the
	example text above which is only there for document formatting purposes.

Προσοχή: Θα χρειαστεί να αφαιρέσετε τη νέα γραμμή μετά το ``:`` στο παραπάνω κείμενο του 
παραδείγματος, το οποίο υπάρχει απλά για λόγους μορφοποίησης.

..
	Now, if you run the script you will see all of the log messages from both the
	echo client and server applications.  You may see that this can be very useful
	in debugging problems.

Αν τρέξετε το σενάριο τώρα, θα παρατηρήσετε όλα τα μηνύματα τόσο από την εφαρμογή του
πελάτη όσο και του εξυπηρετητή. Αυτό μπορεί να αποδειχτεί πολύ χρήσιμο σε 
περιπτώσεις προβλημάτων αποσφαλμάτωσης.

.. sourcecode:: bash

  Waf: Entering directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  Waf: Leaving directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  'build' finished successfully (0.406s)
  UdpEchoServerApplication:UdpEchoServer()
  UdpEchoClientApplication:UdpEchoClient()
  UdpEchoClientApplication:SetDataSize(1024)
  UdpEchoServerApplication:StartApplication()
  UdpEchoClientApplication:StartApplication()
  UdpEchoClientApplication:ScheduleTransmit()
  UdpEchoClientApplication:Send()
  UdpEchoClientApplication:Send(): Sent 1024 bytes to 10.1.1.2
  UdpEchoServerApplication:HandleRead(): Received 1024 bytes from 10.1.1.1
  UdpEchoServerApplication:HandleRead(): Echoing packet
  UdpEchoClientApplication:HandleRead(0x624920, 0x625160)
  UdpEchoClientApplication:HandleRead(): Received 1024 bytes from 10.1.1.2
  UdpEchoServerApplication:StopApplication()
  UdpEchoClientApplication:StopApplication()
  UdpEchoClientApplication:DoDispose()
  UdpEchoServerApplication:DoDispose()
  UdpEchoClientApplication:~UdpEchoClient()
  UdpEchoServerApplication:~UdpEchoServer()

..
	It is also sometimes useful to be able to see the simulation time at which a
	log message is generated.  You can do this by ORing in the prefix_time bit.

Σε κάποιες περιπτώσεις είναι επίσης χρήσιμο να μπορούμε να δούμε τον χρόνο
εξομοίωσης κατά τον οποίο παράχθηκε ένα μήνυμα καταγραφής. Μπορείτε να το 
κάνετε αυτό με τον τελεστή Ή στο ψηφίο prefix_time.

.. sourcecode:: bash

  $ export 'NS_LOG=UdpEchoClientApplication=level_all|prefix_func|prefix_time:
                 UdpEchoServerApplication=level_all|prefix_func|prefix_time'

..
	Again, you will have to remove the newline above.  If you run the script now,
	you should see the following output:

Και εδώ, όπως προηγουμένως, πρέπει να αφαιρέσετε τη νέα γραμμή. Αν τρέξετε το 
σενάριο θα δείτε την παρακάτω έξοδο:

.. sourcecode:: bash

  Waf: Entering directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  Waf: Leaving directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  'build' finished successfully (0.418s)
  0s UdpEchoServerApplication:UdpEchoServer()
  0s UdpEchoClientApplication:UdpEchoClient()
  0s UdpEchoClientApplication:SetDataSize(1024)
  1s UdpEchoServerApplication:StartApplication()
  2s UdpEchoClientApplication:StartApplication()
  2s UdpEchoClientApplication:ScheduleTransmit()
  2s UdpEchoClientApplication:Send()
  2s UdpEchoClientApplication:Send(): Sent 1024 bytes to 10.1.1.2
  2.00369s UdpEchoServerApplication:HandleRead(): Received 1024 bytes from 10.1.1.1
  2.00369s UdpEchoServerApplication:HandleRead(): Echoing packet
  2.00737s UdpEchoClientApplication:HandleRead(0x624290, 0x624ad0)
  2.00737s UdpEchoClientApplication:HandleRead(): Received 1024 bytes from 10.1.1.2
  10s UdpEchoServerApplication:StopApplication()
  10s UdpEchoClientApplication:StopApplication()
  UdpEchoClientApplication:DoDispose()
  UdpEchoServerApplication:DoDispose()
  UdpEchoClientApplication:~UdpEchoClient()
  UdpEchoServerApplication:~UdpEchoServer()

..
	You can see that the constructor for the UdpEchoServer was called at a 
	simulation time of 0 seconds.  This is actually happening before the 
	simulation starts, but the time is displayed as zero seconds.  The same is true
	for the UdpEchoClient constructor message.

Βλέπετε πως έγινε κλήση στο δημιουργό της UdpEchoServer τη χρονική στιγμή 0. 
Αυτό στην πραγματικότητα συμβαίνει πριν ξεκινήσει η εξομοίωση, αλλά ο χρόνος
που εμφανίζεται είναι το 0. Το ίδιο συμβαίνει και για το δημιουργό της UdpEchoClient.

..
	Recall that the ``scratch/first.cc`` script started the echo server 
	application at one second into the simulation.  You can now see that the 
	``StartApplication`` method of the server is, in fact, called at one second.
	You can also see that the echo client application is started at a simulation 
	time of two seconds as we requested in the script.

Θυμηθείτε ότι στο σενάριο ``scratch/first.cc`` η ενεργοποίηση την εφαρμογής του
εξυπηρετητή γίνεται το πρώτο δευτερόλεπτο της εξομοίωσης. Μπορείτε να δείτε
ότι η μέθοδος του εξυπηρετητή ``StartApplication`` όντως καλείται στη χρονική
στιγμή 1. Μπορείτε επίσης να δείτε ότι η εφαρμογή του πελάτη ξεκινάει τη 
χρονική στιγμή 2, όπως ζητήσαμε στο σενάριο.

..
	You can now follow the progress of the simulation from the 
	``ScheduleTransmit`` call in the client that calls ``Send`` to the 
	``HandleRead`` callback in the echo server application.  Note that the 
	elapsed time for the packet to be sent across the point-to-point link is 3.69
	milliseconds.  You see the echo server logging a message telling you that it
	has echoed the packet and then, after another channel delay, you see the echo
	client receive the echoed packet in its ``HandleRead`` method.

Μπορείτε να παρακολουθήσετε την πρόοδο της εξομοίωσης από την κλήση 
``ScheduleTransmit`` στον πελάτη, που καλεί την ``Send`` στην επανάκληση 
``HandleRead`` στην εφαρμογή του εξυπηρετητή. Σημειώστε ότι ο παρερχόμενος 
χρόνος για την αποστολή του πακέτου στη σύνδεση είναι 3.69 δευτερόλεπτα. Μπορείτε 
να δείτε επίσης το μήνυμα καταγραφής του εξυπηρετητή που αναφέρει ότι το πακέτο
έφυγε και στη συνέχεια, μετά από την καθυστέρηση του καναλιού, βλέπετε την
άφιξη του πακέτου στον πελάτη μέσω της μεθόδου ``HandleRead``.

..
	There is a lot that is happening under the covers in this simulation that you
	are not seeing as well.  You can very easily follow the entire process by
	turning on all of the logging components in the system.  Try setting the 
	``NS_LOG`` variable to the following,

Υπάρχουν επίσης πολλά που συμβαίνουν κατά τη διάρκεια της εξομοίωσης τα οποία
δεν εμφανίζονται. Μπορείτε πολύ εύκολα να παρακολουθήσετε ολόκληρη τη διαδικασία
αν θέσετε τη μεταβλητή ``NS_LOG`` στην παρακάτω τιμή,

.. sourcecode:: bash

  $ export 'NS_LOG=*=level_all|prefix_func|prefix_time'

..
	The asterisk above is the logging component wildcard.  This will turn on all 
	of the logging in all of the components used in the simulation.  I won't 
	reproduce the output here (as of this writing it produces 1265 lines of output
	for the single packet echo) but you can redirect this information into a file 
	and look through it with your favorite editor if you like,

Ο αστερίσκος στην παραπάνω εντολή είναι ο τελεστής που δηλώνει ότι θέλουμε να 
ενεργοποιηθεί η καταγραφή σε όλα τα συστατικά στοιχεία που χρησιμοποιούνται
στην εξομοίωση. Δεν θα συμπεριλάβουμε εδώ την έξοδο (μια που αυτή παράγει 1265 
γραμμές απλά για ένα πακέτο), αλλά μπορείτε να ανακατευθύνετε την πληροφορία αυτή
σε ένα αρχείο και να το ανοίξετε με κάποιον επεξεργαστή κειμένου,

.. sourcecode:: bash

  $ ./waf --run scratch/myfirst > log.out 2>&1

..
	I personally use this extremely verbose version of logging when I am presented 
	with a problem and I have no idea where things are going wrong.  I can follow the 
	progress of the code quite easily without having to set breakpoints and step 
	through code in a debugger.  I can just edit up the output in my favorite editor
	and search around for things I expect, and see things happening that I don't 
	expect.  When I have a general idea about what is going wrong, I transition into
	a debugger for a fine-grained examination of the problem.  This kind of output 
	can be especially useful when your script does something completely unexpected.
	If you are stepping using a debugger you may miss an unexpected excursion 
	completely.  Logging the excursion makes it quickly visible.

Προσωπικά χρησιμοποιώ αυτή τη φλύαρη μέθοδο καταγραφής όταν παρουσιάζεται ένα 
πρόβλημα και δεν έχω την παραμικρή ιδέα που βρίσκεται το λάθος. Μπορώ να 
ακολουθήσω τη ροή της εκτέλεσης του κώδικα πολύ εύκολα χωρίς να χρειάζεται να 
θέσω σημεία διακοπής (breakpoints) ή να εξετάσω βήμα-βήμα τον κώδικα στον
αποσφαλματωτή. Μπορώ απλά να ανοίξω την έξοδο στον αγαπημένο μου επεξεργαστή κειμένου
και να ψάξω για πράγματα που περιμένω να συμβαίνουν, αλλά και για πράγματα που
δεν περιμένω να συμβαίνουν. Όταν έχω μια γενική ιδέα του τι πάει λάθος, μεταβαίνω 
σε έναν αποσφαλματωτή για μια πλήρη εξέταση του προβλήματος. Αυτού του είδους η
έξοδος μπορεί να είναι ιδιαίτερα χρήσιμη όταν το σενάριο κάνει κάτι τελείως
απρόβλεπτο. Αν χρησιμοποιήσετε απλά τον αποσφαλματωτή, μπορείτε να παραβλέψετε τελείως
μια απρόβλεπτη συμπεριφορά. Με την καταγραφή μπορούμε να την εντοπίσουμε γρήγορα.

..
	Adding Logging to your Code
	+++++++++++++++++++++++++++
	You can add new logging to your simulations by making calls to the log 
	component via several macros.  Let's do so in the ``myfirst.cc`` script we
	have in the ``scratch`` directory.
	
	Recall that we have defined a logging component in that script:

Προσθήκη Καταγραφής σε Κώδικα
+++++++++++++++++++++++++++++

Μπορείτε να προσθέσετε νέες καταγραφές στις εξομοιώσεις σας καλώντας το στοιχείο
καταγραφής μέσω διαφόρων μακροεντολών. Ας το επιχειρήσουμε στο σενάριο εξομοίωσης
``myfirst.cc`` που βρίσκεται στον φάκελο ``scratch``.

Θυμηθείτε ότι έχουμε ορίσει ένα στοιχείο καταγραφής σε εκείνο το σενάριο:

::

  NS_LOG_COMPONENT_DEFINE ("FirstScriptExample");
  
..
	You now know that you can enable all of the logging for this component by
	setting the ``NS_LOG`` environment variable to the various levels.  Let's
	go ahead and add some logging to the script.  The macro used to add an 
	informational level log message is ``NS_LOG_INFO``.  Go ahead and add one 
	(just before we start creating the nodes) that tells you that the script is 
	"Creating Topology."  This is done as in this code snippet,

	Open ``scratch/myfirst.cc`` in your favorite editor and add the line,

Γνωρίζετε τώρα ότι μπορείτε να ενεργοποιήσετε όλες τις δυνατές καταγραφές
για αυτό το στοιχείο, θέτοντας τη μεταβλητή περιβάλλοντος ``NS_LOG`` σε 
κάποιο επίπεδο. Ας προχωρήσουμε στην προσθήκη καταγραφής στο σενάριο. Η
μακροεντολή που προσθέτει καταγραφή σε επίπεδο πληροφοριακών μηνυμάτων 
είναι η ``NS_LOG_INFO``. Θέλουμε να προσθέσουμε ένα μήνυμα (πριν αρχίσουμε 
να δημιουργούμε κόμβους) που αναφέρει ότι το σενάριο δημιουργεί μια τοπολογία 
"Creating Topology". Αυτό γίνεται όπως δείχνουμε στον παρακάτω κώδικα,

Ανοίξτε το ``scratch/myfirst.cc`` σε έναν επεξεργαστή κειμένου και προσθέστε
τη γραμμή,
::

  NS_LOG_INFO ("Creating Topology");

..
	right before the lines,
	
αμέσως πριν από τις γραμμές,

::

  NodeContainer nodes;
  nodes.Create (2);

..
	Now build the script using waf and clear the ``NS_LOG`` variable to turn 
	off the torrent of logging we previously enabled:

Τώρα ας οικοδομήσουμε το σενάριο χρησιμοποιώντας το waf και καθαρίζοντας τη
μεταβλητή ``NS_LOG`` ώστε να απενεργοποιήσουμε το torrent της καταγραφής που 
είχαμε προηγουμένως ενεργοποιήσει:

.. sourcecode:: bash

  $ ./waf
  $ export NS_LOG=

..
	Now, if you run the script, 

Αν τρέξετε το σενάριο τώρα,

.. sourcecode:: bash

  $ ./waf --run scratch/myfirst

..
	you will ``not`` see your new message since its associated logging 
	component (``FirstScriptExample``) has not been enabled.  In order to see your
	message you will have to enable the ``FirstScriptExample`` logging component
	with a level greater than or equal to ``NS_LOG_INFO``.  If you just want to 
	see this particular level of logging, you can enable it by,

δεν θα δείτε το νέο μήνυμα, αφού το σχετικό στοιχείο καταγραφής 
(``FirstScriptExample``) δεν έχει ενεργοποιηθεί. Για να δείτε το μήνυμά σας 
θα πρέπει να το ενεργοποιήσετε με επίπεδο καταγραφής μεγαλύτερο ή ίσο με 
``NS_LOG_INFO``. Αν θέλετε απλά να δείτε το συγκεκριμένο επίπεδο καταγραφής,
μπορείτε να το ενεργοποιήσετε ως εξής,

.. sourcecode:: bash

  $ export NS_LOG=FirstScriptExample=info

..
	If you now run the script you will see your new "Creating Topology" log
	message,

Αν τρέξετε τώρα το σενάριο, θα δείτε το μήνυμα καταγραφής "Creating Topology",

.. sourcecode:: bash

  Waf: Entering directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  Waf: Leaving directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  'build' finished successfully (0.404s)
  Creating Topology
  Sent 1024 bytes to 10.1.1.2
  Received 1024 bytes from 10.1.1.1
  Received 1024 bytes from 10.1.1.2

..
	Using Command Line Arguments
	***************************

Χρησιμοποιώντας ορίσματα γραμμής εντολών
****************************************

.. _Attribute:

..
	Overriding Default Attributes
	+++++++++++++++++++++++++++++

Παρακάμπτοντας Προκαθορισμένα Ορίσματα
++++++++++++++++++++++++++++++++++++++
..
	Another way you can change how |ns3| scripts behave without editing
	and building is via *command line arguments.*  We provide a mechanism to 
	parse command line arguments and automatically set local and global variables
	based on those arguments.

Ένας άλλος τρόπος που μπορείτε να αλλάξετε τον τρόπο που τα |ns3| σενάρια 
συμπεριφέρονται, χωρίς να χρειάζεται επεξεργασία και οικοδόμηση, είναι μέσω 
*ορισμάτων γραμμής εντολών*. Παρέχουμε ένα μηχανισμό που να αναλύσει τα ορίσματα 
γραμμής εντολών και αυτόματα θέτει τις τοπικές και καθολικές μεταβλητές με βάση 
τα ορίσματα αυτά.

..
	The first step in using the command line argument system is to declare the
	command line parser.  This is done quite simply (in your main program) as
	in the following code,

Το πρώτο βήμα για τη χρήση του συστήματος ορισμάτων γραμμής εντολών, είναι 
να δηλώσουμε τον αναλυτή γραμμής εντολών. Αυτό γίνεται πολύ απλά (στο κύριο 
πρόγραμμα σας) όπως στον ακόλουθο κώδικα,

::

  int
  main (int argc, char *argv[])
  {
    ...  

    CommandLine cmd;
    cmd.Parse (argc, argv);

    ...
  }

..
	This simple two line snippet is actually very useful by itself.  It opens the
	door to the |ns3| global variable and ``Attribute`` systems.  Go 
	ahead and add that two lines of code to the ``scratch/myfirst.cc`` script at
	the start of ``main``.  Go ahead and build the script and run it, but ask 
	the script for help in the following way,

Αυτό το απλό απόσπασμα δύο γραμμών είναι πραγματικά πολύ χρήσιμο από μόνο του.
Ανοίγει την πόρτα για τα συστήματα καθολικών μεταβλητών και ``Attributes`` του 
|ns3|. Προσθέστε αυτές τις δύο γραμμές κώδικα στο σενάριο ``scratch/first.cc`` 
στην αρχή της ``main``. Οικοδομήστε το σενάριο και τρέξτε το, αλλά ζητήστε 
βοήθεια από το σενάριο με τον ακόλουθο τρόπο,
	
.. sourcecode:: bash

  $ ./waf --run "scratch/myfirst --PrintHelp"

..
	This will ask Waf to run the ``scratch/myfirst`` script and pass the command
	line argument ``--PrintHelp`` to the script.  The quotes are required to 
	sort out which program gets which argument.  The command line parser will
	now see the ``--PrintHelp`` argument and respond with,

Αυτό θα ζητήσει από τον Waf να τρέξει το σενάριο ``scratch/myfirst`` και 
να περάσει το όρισμα γραμμής εντολών ``--PrintHelp`` στο σενάριο. Τα εισαγωγικά
απαιτούνται για να ορίσουμε ποιο από τα προγράμματα παίρνει το κάθε όρισμα. Ο 
αναλυτής της γραμμής εντολών θα δει το όρισμα ``--PrintHelp`` και θα αποκριθεί 
με,

.. sourcecode:: bash

  Waf: Entering directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  Waf: Leaving directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  'build' finished successfully (0.413s)
  TcpL4Protocol:TcpStateMachine()
  CommandLine:HandleArgument(): Handle arg name=PrintHelp value=
  --PrintHelp: Print this help message.
  --PrintGroups: Print the list of groups.
  --PrintTypeIds: Print all TypeIds.
  --PrintGroup=[group]: Print all TypeIds of group.
  --PrintAttributes=[typeid]: Print all attributes of typeid.
  --PrintGlobals: Print the list of globals.

..
	Let's focus on the ``--PrintAttributes`` option.  We have already hinted
	at the |ns3| ``Attribute`` system while walking through the 
	``first.cc`` script.  We looked at the following lines of code,

Ας επικεντρωθούμε στην επιλογή ``--PrintAttributes``. Έχουμε ήδη υπαινιχθεί 
για το σύστημα ``Ορισμάτων`` |ns3| ενώ ακολουθούσαμε βήμα-βήμα το σενάριο
``first.cc``. Αν κοιτάξουμε τις ακόλουθες γραμμές κώδικα,
::

    PointToPointHelper pointToPoint;
    pointToPoint.SetDeviceAttribute ("DataRate", StringValue ("5Mbps"));
    pointToPoint.SetChannelAttribute ("Delay", StringValue ("2ms"));

..
	and mentioned that ``DataRate`` was actually an ``Attribute`` of the 
	``PointToPointNetDevice``.  Let's use the command line argument parser
	to take a look at the ``Attributes`` of the PointToPointNetDevice.  The help
	listing says that we should provide a ``TypeId``.  This corresponds to the
	class name of the class to which the ``Attributes`` belong.  In this case it
	will be ``ns3::PointToPointNetDevice``.  Let's go ahead and type in,

παρατηρούμε ότι το ``DataRate`` είναι στην πραγματικότητα ένα ``Όρισμα`` 
του `PointToPointNetDevice``. Ας χρησιμοποιήσουμε τον αναλυτή ορισμάτων 
γραμμής εντολών για να παρατήσουμε τα ``Attributes`` του PointToPointNetDevice. 
Η λίστα βοήθειας αναφέρει ότι πρέπει να παρέχουμε ένα ``TypeId``. Αυτό 
αντιστοιχεί στο όνομα της κλάσης στην οποία ανήκουν τα ``Attributes``. Σε 
αυτή την περίπτωση θα είναι ``ns3::PointToPointNetDevice``. Αν το τυπώσουμε,

.. sourcecode:: bash

  $ ./waf --run "scratch/myfirst --PrintAttributes=ns3::PointToPointNetDevice"

..
	The system will print out all of the ``Attributes`` of this kind of net device.
	Among the ``Attributes`` you will see listed is,

Το σύστημα θα τυπώσει όλα τα ``Attributes`` αυτού του είδους συσκευών δικτύου.
Μεταξύ των ``Attributes`` θα δείτε είναι και το ακόλουθο,
	
.. sourcecode:: bash

  --ns3::PointToPointNetDevice::DataRate=[32768bps]:
    The default data rate for point to point links

..
	This is the default value that will be used when a ``PointToPointNetDevice``
	is created in the system.  We overrode this default with the ``Attribute``
	setting in the ``PointToPointHelper`` above.  Let's use the default values 
	for the point-to-point devices and channels by deleting the 
	``SetDeviceAttribute`` call and the ``SetChannelAttribute`` call from 
	the ``myfirst.cc`` we have in the scratch directory.

	Your script should now just declare the ``PointToPointHelper`` and not do 
	any ``set`` operations as in the following example,

Αυτή είναι η προεπιλεγμένη τιμή που θα χρησιμοποιηθεί όταν δημιουργείται 
στο σύστημα μία ``PointToPointNetDevice``. Εμείς παρακάμψαμε αυτή την 
προεπιλογή με την ρύθμιση ``Attribute`` στο ``PointToPointHelper``. Ας 
χρησιμοποιήσουμε τις προεπιλεγμένες τιμές για τις συσκευές point-to-point 
και τα κανάλια με τη διαγραφή των κλήσεων ``SetDeviceAttribute`` και 
``SetChannelAttribute`` από το ``myfirst.cc`` στον κατάλογο scratch. 

Το σενάριό σας πρέπει τώρα να δηλώσει το ``PointToPointHelper`` και να μην 
κάνει κάποια ``set`` ενέργεια όπως στο ακόλουθο παράδειγμα,

::

  ...

  NodeContainer nodes;
  nodes.Create (2);

  PointToPointHelper pointToPoint;

  NetDeviceContainer devices;
  devices = pointToPoint.Install (nodes);

  ...

..
	Go ahead and build the new script with Waf (``./waf``) and let's go back 
	and enable some logging from the UDP echo server application and turn on the 
	time prefix.

Ας οικοδομήσουμε το νέο σενάριο με Waf (``./waf``) επιτρέποντας κάποια 
καταγραφή από την εφαρμογή διακομιστή UDP echo και ενεργοποιώντας το πρόθεμα
ώρας.
	
.. sourcecode:: bash

  $ export 'NS_LOG=UdpEchoServerApplication=level_all|prefix_time'

..
	If you run the script, you should now see the following output,

Αν τρέξουμε το σενάριο, θα πρέπει να δούμε την ακόλουθη έξοδο,
	
.. sourcecode:: bash

  Waf: Entering directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  Waf: Leaving directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  'build' finished successfully (0.405s)
  0s UdpEchoServerApplication:UdpEchoServer()
  1s UdpEchoServerApplication:StartApplication()
  Sent 1024 bytes to 10.1.1.2
  2.25732s Received 1024 bytes from 10.1.1.1
  2.25732s Echoing packet
  Received 1024 bytes from 10.1.1.2
  10s UdpEchoServerApplication:StopApplication()
  UdpEchoServerApplication:DoDispose()
  UdpEchoServerApplication:~UdpEchoServer()

..
	Recall that the last time we looked at the simulation time at which the packet
	was received by the echo server, it was at 2.00369 seconds.

Θυμηθείτε ότι την τελευταία φορα που είδαμε το χρόνο εξομοίωσης όπου το
πακέτο παρελήφθηκε από τον διακομιστή, ήταν στα 2.00369 δευτερόλεπτα.
	
.. sourcecode:: bash

  2.00369s UdpEchoServerApplication:HandleRead(): Received 1024 bytes from 10.1.1.1

..
	Now it is receiving the packet at 2.25732 seconds.  This is because we just dropped
	the data rate of the ``PointToPointNetDevice`` down to its default of 
	32768 bits per second from five megabits per second.

	If we were to provide a new ``DataRate`` using the command line, we could 
	speed our simulation up again.  We do this in the following way, according to
	the formula implied by the help item:

Τώρα λαμβάνει το πακέτο στα 2.25732 δευτερόλεπτα. Η αλαγή αυτή οφείλεται στη 
μείωση του ρυθμού μετάδοσης του ``PointToPointNetDevice`` από τα 5 megabits ανά 
δευτερόλεπτο στην προκαθορισμένη τιμή των 32768 bits ανά δευτερόλεπτο.

Αν παρείχαμε το νέο ``DataRate`` μέσω της γραμμής εντολών, θα μπορούσαμε 
να επιταχύνουμε την εξομοίωσή μας και πάλι. Αυτό γίνεται με τον ακόλουθο 
τρόπο,
	
.. sourcecode:: bash

  $ ./waf --run "scratch/myfirst --ns3::PointToPointNetDevice::DataRate=5Mbps"

..
	This will set the default value of the ``DataRate`` ``Attribute`` back to 
	five megabits per second.  Are you surprised by the result?  It turns out that
	in order to get the original behavior of the script back, we will have to set 
	the speed-of-light delay of the channel as well.  We can ask the command line 
	system to print out the ``Attributes`` of the channel just like we did for
	the net device:

Αυτό θα ορίσει την προκαθορισμένη τιμή του ``DataRate`` ``Attribute`` πάλι σε 
5 megabits ανά δευτερόλεπτο. Εκπλαγήκατε από το αποτέλεσμα; Φαίνεται ότι για 
να επαναφέρουμε την αρχική συμπεριφορά του σεναρίου, θα πρέπει να ρυθμίσουμε 
την καθυστέρηση του καναλιού στην ταχύτητα του φωτός. Μπορούμε να ζητήσουμε 
από το σύστημα γραμμής εντολών να εκτυπώσει τα ``Attributes`` του καναλιού, 
ακριβώς όπως κάναμε για την δικτυακή συσκευή:
	
.. sourcecode:: bash

  $ ./waf --run "scratch/myfirst --PrintAttributes=ns3::PointToPointChannel"

..
	We discover the ``Delay`` ``Attribute`` of the channel is set in the following
	way:

Ανακαλύπτουμε ότι το ``Delay`` ``Attribute`` του καναλιού είναι ενεργοποιημένο 
με τον ακόλουθο τρόπο:
	
.. sourcecode:: bash

  --ns3::PointToPointChannel::Delay=[0ns]:
    Transmission delay through the channel

..
	We can then set both of these default values through the command line system,

Μπορούμε λοιπόν να θέσουμε και τις δύο προκαθορισμένες τιμές μέσω του 
συστήματος γραμμής εντολών,
	
.. sourcecode:: bash

  $ ./waf --run "scratch/myfirst
    --ns3::PointToPointNetDevice::DataRate=5Mbps
    --ns3::PointToPointChannel::Delay=2ms"

..
	in which case we recover the timing we had when we explicitly set the
	``DataRate`` and ``Delay`` in the script:

όπου επαναφέρουμε τον χρονισμό που είχαμε όταν θέσαμε το ``DataRate`` και το
``Delay`` στο σενάριο:
	
.. sourcecode:: bash

  Waf: Entering directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  Waf: Leaving directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  'build' finished successfully (0.417s)
  0s UdpEchoServerApplication:UdpEchoServer()
  1s UdpEchoServerApplication:StartApplication()
  Sent 1024 bytes to 10.1.1.2
  2.00369s Received 1024 bytes from 10.1.1.1
  2.00369s Echoing packet
  Received 1024 bytes from 10.1.1.2
  10s UdpEchoServerApplication:StopApplication()
  UdpEchoServerApplication:DoDispose()
  UdpEchoServerApplication:~UdpEchoServer()

..
	Note that the packet is again received by the server at 2.00369 seconds.  We 
	could actually set any of the ``Attributes`` used in the script in this way.
	In particular we could set the ``UdpEchoClient Attribute MaxPackets`` 
	to some other value than one.

	How would you go about that?  Give it a try.  Remember you have to comment 
	out the place we override the default ``Attribute`` and explicitly set 
	``MaxPackets`` in the script.  Then you have to rebuild the script.  You 
	will also have to find the syntax for actually setting the new default attribute
	value using the command line help facility.  Once you have this figured out 
	you should be able to control the number of packets echoed from the command 
	line.  Since we're nice folks, we'll tell you that your command line should 
	end up looking something like,

Σημειώστε ότι το πακέτο λαμβάνεται και πάλι από το διακομιστή στα 2.00369 
δευτερόλεπτα. Στην ουσία θα μπορούσαμε να ορίσουμε με αυτόν τον τρόπο οποιαδήποτε 
από τα ``Attributes`` τα οποία χρησιμοποιούνται στο σενάριο. Ειδικότερα, θα μπορούσαμε
να θέσουμε το ``UdpEchoClient Attribute MaxPackets`` σε κάποια διαφορετική τιμή από 
τη μονάδα.

Πώς θα το πραγματοποιούσατε αυτό; Κάντε μια δοκιμή. Να θυμάστε ότι πρέπει να 
σχολιάσετε το μέρος που αντικαθιστά το προεπιλεγμένο ``Attribute`` και ορίσετε 
ρητά το ``MaxPackets`` στο σενάριο. Στη συνέχεια θα πρέπει να ξαναοικοδομήσετε
το σενάριο. Θα πρέπει επίσης να βρείτε τη σύνταξη για να ορίσετε τη νέα προεπιλεγμένη 
τιμή της ιδιότητας, χρησιμοποιώντας τη βοήθεια της γραμμής εντολών. Μόλις έχετε 
καταλάβει αυτό το βήμα, θα πρέπει να είστε σε θέση να ελέγχετε τον αριθμό των πακέτων 
που αντανακλώνται από τη γραμμή εντολών. Μιας που είμαστε καλά παιδιά, θα σας πούμε 
ότι η γραμμή εντολών σας θα πρέπει να μοιάζει κάπως έτσι,
	
.. sourcecode:: bash

  $ ./waf --run "scratch/myfirst 
    --ns3::PointToPointNetDevice::DataRate=5Mbps 
    --ns3::PointToPointChannel::Delay=2ms 
    --ns3::UdpEchoClient::MaxPackets=2"

..
	Hooking Your Own Values
	+++++++++++++++++++++++
	You can also add your own hooks to the command line system.  This is done
	quite simply by using the ``AddValue`` method to the command line parser.

	Let's use this facility to specify the number of packets to echo in a 
	completely different way.  Let's add a local variable called ``nPackets``
	to the ``main`` function.  We'll initialize it to one to match our previous 
	default behavior.  To allow the command line parser to change this value, we
	need to hook the value into the parser.  We do this by adding a call to 
	``AddValue``.  Go ahead and change the ``scratch/myfirst.cc`` script to
	start with the following code,

Συνδέοντας τις δικές σας τιμές
++++++++++++++++++++++++++++++

Μπορείτε να προσθέσετε τις δικές σας συνδέσεις στο σύστημα γραμμής εντολών. 
Αυτό γίνεται με έναν απλό τρόπο, απλά χρησιμοποιώντας τη μέθοδο ``AddValue``
στον αναλυτή γραμμής εντολών.

Ας χρησιμοποιήσουμε αυτή τη λειτουργία για να ορίσουμε με έναν τελείως 
διαφορετικό τρόπο τον αριθμό των πακέτων που αντανακλώνται. Ας προσθέσουμε 
στη συνάρτηση ``main`` μία τοπική μεταβλητή με το όνομα ``nPackets``. Θα 
την αρχικοποιήσουμε στην τιμή 1 για να ταυτιστεί με την προηγούμενη
προκαθορισμένη τιμή. Για να επιτρέψουμε στον αναλυτή γραμμής εντολών να 
τροποποιήσει την τιμή αυτή, πρέπει να συνδέσουμε την τιμή στον αναλυτή. 
Αυτό το κάνουμε με την προσθήκη μιας κλήσης στην ``AddValue``. Αλλάξτε 
το σενάριο ``scratch/myfirst.cc`` έτσι ώστε να αρχίζει με αυτόν τον 
κώδικα,
	
::

  int
  main (int argc, char *argv[])
  {
    uint32_t nPackets = 1;

    CommandLine cmd;
    cmd.AddValue("nPackets", "Number of packets to echo", nPackets);
    cmd.Parse (argc, argv);

    ...

..
	Scroll down to the point in the script where we set the ``MaxPackets``
	``Attribute`` and change it so that it is set to the variable ``nPackets``
	instead of the constant ``1`` as is shown below.

Κυλήστε το σενάριο προς τα κάτω μέχρι το σημείο όπου θέτουμε το όρισμα 
``MaxPackets`` και αλλάξτε το έτσι ώστε να δείχνει στη μεταβλητή ``nPackets``
αντί να παίρνει την τιμή 1 όπως δείχνουμε παρακάτω,
	
::

  echoClient.SetAttribute ("MaxPackets", UintegerValue (nPackets));

..
	Now if you run the script and provide the ``--PrintHelp`` argument, you 
	should see your new ``User Argument`` listed in the help display.

	Try,

Τώρα αν τρέξετε το σενάριο και παρέχετε το όρισμα ``--PrintHelp``, θα 
μπορείτε να δείτε στην οθόνη βοήθειας το νέο σας ``User Argument``.

.. sourcecode:: bash

  $ ./waf --run "scratch/myfirst --PrintHelp"

.. sourcecode:: bash

  Waf: Entering directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  Waf: Leaving directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  'build' finished successfully (0.403s)
  --PrintHelp: Print this help message.
  --PrintGroups: Print the list of groups.
  --PrintTypeIds: Print all TypeIds.
  --PrintGroup=[group]: Print all TypeIds of group.
  --PrintAttributes=[typeid]: Print all attributes of typeid.
  --PrintGlobals: Print the list of globals.
  User Arguments:
      --nPackets: Number of packets to echo

..
	If you want to specify the number of packets to echo, you can now do so by
	setting the ``--nPackets`` argument in the command line,

Αν θέλετε να καθορίσετε τον αριθμό των πακέτων που αντανακλώνται, μπορείτε 
να θέσετε το όρισμα ``--nPackets`` στην γραμμή εντολών,
	
.. sourcecode:: bash

  $ ./waf --run "scratch/myfirst --nPackets=2"

..
	You should now see

Θα πρέπει να εμφανίζεται τώρα
	
.. sourcecode:: bash

  Waf: Entering directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  Waf: Leaving directory `/home/craigdo/repos/ns-3-allinone/ns-3-dev/build'
  'build' finished successfully (0.404s)
  0s UdpEchoServerApplication:UdpEchoServer()
  1s UdpEchoServerApplication:StartApplication()
  Sent 1024 bytes to 10.1.1.2
  2.25732s Received 1024 bytes from 10.1.1.1
  2.25732s Echoing packet
  Received 1024 bytes from 10.1.1.2
  Sent 1024 bytes to 10.1.1.2
  3.25732s Received 1024 bytes from 10.1.1.1
  3.25732s Echoing packet
  Received 1024 bytes from 10.1.1.2
  10s UdpEchoServerApplication:StopApplication()
  UdpEchoServerApplication:DoDispose()
  UdpEchoServerApplication:~UdpEchoServer()

..
	You have now echoed two packets.  Pretty easy, isn't it?

	You can see that if you are an |ns3| user, you can use the command 
	line argument system to control global values and ``Attributes``.  If you are
	a model author, you can add new ``Attributes`` to your ``Objects`` and 
	they will automatically be available for setting by your users through the
	command line system.  If you are a script author, you can add new variables to 
	your scripts and hook them into the command line system quite painlessly.

Έχετε αντανακλάσει τώρα δύο πακέτα. Φαίνεται ιδιαίτερα εύκολο, έτσι δεν είναι;

Αν είστε ένας χρήστης |ns3| λοιπόν, μπορείτε να χρησιμοποιείτε το σύστημα 
ορισμάτων γραμμής εντολών για να ελέγχετε τα ``Attributes`` και τις μεταβλητές 
συστήματος. Αν είστε ο συγγραφέας ενός μοντέλου, μπορείτε να προσθέτετε νέα 
``Attributes`` στα ``Objects`` σας και αυτά θα είναι αυτόματα διαθέσιμα στους 
χρήστες σας για να θέσουν τιμές μέσω του συστήματος γραμμής εντολών. Αν είστε 
ο συγγραφέας ενός σεναρίου, μπορείτε να προσθέτετε νέες μεταβλητές στα 
σενάριά σας και να τις συνδέετε στο σύστημα γραμμής εντολών χωρίς ιδιαίτερο 
κόπο.

..
	Using the Tracing System
	************************

	The whole point of simulation is to generate output for further study, and 
	the |ns3| tracing system is a primary mechanism for this.  Since 
	|ns3| is a C++ program, standard facilities for generating output 
	from C++ programs could be used:  

.. _UsingTracingSystem:

Χρησιμοποιώντας το Σύστημα Ιχνηλασίας
*************************************

Το όλο νόημα της εξομοίωσης είναι να παράγουμε έξοδο για μελλοντικές μελέτες, 
και το σύστημα ιχνηλασίας του |ns3| είναι ένας πρωταρχικός μηχανισμός για 
το σκοπό αυτό. Αφού το |ns3| είναι ένα πρόγραμμα σε γλώσσα C++, μπορούν 
να χρησιμοποιηθούν τυποποιημένες λειτουργίες για τη παραγωγή εξόδου από 
προγράμματα C++.
	
::

  #include <iostream>
  ...
  int main ()
  {
    ...
    std::cout << "The value of x is " << x << std::endl;
    ...
  } 

..
	You could even use the logging module to add a little structure to your 
	solution.  There are many well-known problems generated by such approaches
	and so we have provided a generic event tracing subsystem to address the 
	issues we thought were important.

	The basic goals of the |ns3| tracing system are:

	* For basic tasks, the tracing system should allow the user to generate 
	standard tracing for popular tracing sources, and to customize which objects
	generate the tracing;
	* Intermediate users must be able to extend the tracing system to modify
	the output format generated, or to insert new tracing sources, without 
	modifying the core of the simulator;
	* Advanced users can modify the simulator core to add new tracing sources
	and sinks.

Θα μπορούσατε ακόμη και να χρησιμοποιήσετε τη μονάδα καταγραφής για να 
προσθέσετε κάποια δομή στη λύση σας. Υπάρχουν πολλά γνωστά προβλήματα που 
δημιουργούνται από τέτοιες προσεγγίσεις και έτσι παρέχουμε ένα υποσύστημα 
ιχνηλασίας γεγονότων για να αντιμετωπίσουμε τα θέματα που θεωρήσαμε ότι 
ήταν σημαντικά.

Οι βασικοί στόχοι του συστήματος ανίχνευσης του |ns3| είναι:

* 	Για τα βασικά του καθήκοντα, το σύστημα ανίχνευσης θα πρέπει να επιτρέπει
	στο χρήστη να παράγει τυποποιημένη ιχνηλασία για δημοφιλείς πηγές ιχνηλασίας,
	και να προσαρμόζει ποια αντικείμενα δημιουργούν την ιχνηλασία;
*	Οι μέσοι χρήστες θα πρέπει να είναι σε θέση να επεκτείνουν το σύστημα
	ιχνηλασίας ώστε να τροποποιούν τη μορφή εξόδου που παράγεται, ή να εισάγουν
	νέες πηγές ιχνηλασίας, χωρίς να αλλάζουν τον πυρήνα του προσομοιωτή;
*	Οι προχωρημένοι χρήστες μπορούν να τροποποιήσουν τον πυρήνα προσομοιωτή
	ώστε να προσθέτουν νέες πηγές και καταβόθρες ιχνηλασίας. 


..
	The |ns3| tracing system is built on the concepts of independent 
	tracing sources and tracing sinks, and a uniform mechanism for connecting
	sources to sinks.  Trace sources are entities that can signal events that
	happen in a simulation and provide access to interesting underlying data. 
	For example, a trace source could indicate when a packet is received by a net
	device and provide access to the packet contents for interested trace sinks.

	Trace sources are not useful by themselves, they must be "connected" to
	other pieces of code that actually do something useful with the information 
	provided by the sink.  Trace sinks are consumers of the events and data
	provided by the trace sources.  For example, one could create a trace sink 
	that would (when connected to the trace source of the previous example) print 
	out interesting parts of the received packet.

	The rationale for this explicit division is to allow users to attach new
	types of sinks to existing tracing sources, without requiring editing and 
	recompilation of the core of the simulator.  Thus, in the example above, 
	a user could define a new tracing sink in her script and attach it to an 
	existing tracing source defined in the simulation core by editing only the 
	user script.

	In this tutorial, we will walk through some pre-defined sources and sinks and
	show how they may be customized with little user effort.  See the ns-3 manual
	or how-to sections for information on advanced tracing configuration including
	extending the tracing namespace and creating new tracing sources.

Το σύστημα ιχνηλασίας |ns3| είναι χτισμένο στις έννοιες των ανεξάρτητων πηγών 
και καταβοθρών ιχνηλασίας, και ενός ενιαίου μηχανισμού για τη σύνδεση πηγών σε 
καταβόθρες. Οι πηγές ίχνους είναι οντότητες οι οποίες μπορούν να σηματοδοτήσουν 
γεγονότα που συμβαίνουν σε μια εξομοίωση και παρέχουν πρόσβαση σε ενδιαφέροντα
δεδομένα. Για παράδειγμα, μια πηγή ίχνους θα μπορούσε να υποδείξει πότε ένα πακέτο 
λαμβάνεται από μία δικτυακή συσκευή και να παρέχει πρόσβαση στα περιεχόμενα του 
πακέτου για τους ενδιαφερόμενες καταβόθρες ίχνους.

Οι πηγές ίχνους δεν είναι χρήσιμες από μόνες τους, θα πρέπει να είναι "συνδεδεμένες" 
με άλλα κομμάτια κώδικα που κάνουν πραγματικά κάτι χρήσιμο με τις πληροφορίες που 
παρέχονται από την καταβόθρα. Οι καταβόθρες ίχνους είναι οι καταναλωτές των 
γεγονότων και των δεδομένων  που παρέχονται από τις πηγές ίχνους. Για παράδειγμα, 
κάποιος θα μπορούσε να δημιουργήσει μία καταβόθρα ίχνους που θα εκτύπωνε ενδιαφέροντα 
μέρη του ληφθέντος πακέτου (όταν συνδέεται με την πηγή ίχνους του προηγούμενου 
παραδείγματος).

Το σκεπτικό για αυτή την ρητή διαίρεση είναι να επιτρέψει στους χρήστες να 
επισυνάπτουν νέους τύπους καταβοθρών στις υπάρχουσες πηγές ιχνηλασίας, χωρίς να 
απαιτείται επεξεργασία και επαναμεταγλωτισμός του πυρήνα του εξομοιωτή. Έτσι, στο 
παραπάνω παράδειγμα, ο χρήστης μπορεί με την επεξεργασία μόνο του σεναρίου του χρήστη 
να καθορίσει μια νέα καταβόθρα ιχνηλασίας στο σενάριό του και να το επισυνάψει σε 
μια υπάρχουσα πηγή ιχνηλασίας που ορίζεται στον πυρήνα εξομοίωσης.

Σε αυτόν τον οδηγό, θα εξετάσουμε κάποιες προκαθορισμένες πηγές και καταβόθρες και
θα δείξουμε πως μπορούν να προσαρμοστούν με μια μικρή προσπάθεια. Δείτε το εγχειρίδιο 
ns-3 ή τις ενότητες how-to για πληροφορίες σχετικά με τη διαμόρφωση προηγμένης 
ιχνηλασίας, συμπεριλαμβανομένων της επέκτασης του χώρου ονομάτων ιχνηλασίας και της 
δημιουργίας νέων πηγών ιχνηλασίας.

..
	ASCII Tracing
	+++++++++++++
	|ns3| provides helper functionality that wraps the low-level tracing
	system to help you with the details involved in configuring some easily 
	understood packet traces.  If you enable this functionality, you will see
	output in a ASCII files --- thus the name.  For those familiar with 
	|ns2| output, this type of trace is analogous to the ``out.tr``
	generated by many scripts.

	Let's just jump right in and add some ASCII tracing output to our 
	``scratch/myfirst.cc`` script.  Right before the call to 
	``Simulator::Run ()``, add the following lines of code:

Ιχνηλασία Ascii
+++++++++++++++

Το |ns3| παρέχει λειτουργικότητα βοήθειας που συμπεριλαμβάνει το σύστημα 
ιχνηλασίας χαμηλού επιπέδου για να σας βοηθήσει με τις λεπτομέρειες που 
εμπλέκονται στη διαμόρφωση μερικών ευκατανόητων ιχνών πακέτων. Αν ενεργοποιήσετε 
αυτή τη λειτουργία, θα δείτε την εξόδο σε αρχεία ASCII --- εξού και το όνομα. 
Για όσους είναι εξοικειωμένοι με την έξοδο του |ns2|, το ίχνος αυτού του είδους 
είναι ανάλογο με το ``out.tr`` που παράγεται από πολλά σενάρια.

Ας πάμε κατευθείαν να προσθέσουμε κάποια έξοδο ιχνηλασίας ASCII στο σενάριό μας
``scratch/myfirst.cc``. Ακριβώς πριν από την κλήση προς ``Simulator::Run ()``, 
προσθέστε τις ακόλουθες γραμμές κώδικα:
	
::

  AsciiTraceHelper ascii;
  pointToPoint.EnableAsciiAll (ascii.CreateFileStream ("myfirst.tr"));

..
	Like in many other |ns3| idioms, this code uses a  helper object to 
	help create ASCII traces.  The second line contains two nested method calls.  
	The "inside" method, ``CreateFileStream()`` uses an unnamed object idiom
	to create a file stream object on the stack (without an object  name) and pass
	it down to the called method.  We'll go into this more in the future, but all
	you have to know at this point is that you are creating an object representing
	a file named "myfirst.tr" and passing it into ``ns-3``.  You are telling 
	``ns-3`` to deal with the lifetime issues of the created object and also to 
	deal with problems caused by a little-known (intentional) limitation of C++ 
	ofstream objects relating to copy constructors.

	The outside call, to ``EnableAsciiAll()``, tells the helper that you 
	want to enable ASCII tracing on all point-to-point devices in your simulation; 
	and you want the (provided) trace sinks to write out information about packet 
	movement in ASCII format.

	For those familiar with |ns2|, the traced events are equivalent to 
	the popular trace points that log "+", "-", "d", and "r" events.

	You can now build the script and run it from the command line:

Όπως και σε πολλά άλλα ιδιώματα |ns3|, αυτός ο κώδικας χρησιμοποιεί ένα βοηθητικό 
αντικείμενο για να δημιουργήσει ίχνη ASCII. Η δεύτερη γραμμή περιέχει δύο ένθετες 
κλήσεις μεθόδων. Η "εσωτερική" μέθοδος, ``CreateFileStream()`` χρησιμοποιεί ένα 
ανώνυμο ιδίωμα αντικειμένου για να δημιουργήσει ένα αντικείμενο ρεύματος αρχείου 
στη στοίβα (χωρίς όνομα αντικειμένου) και να το περάσει στην καλούμενη μέθοδο. 
Θα επιστρέψουμε στο σημείο αυτό αργότερα, αλλά αυτό που πρέπει να ξέρετε στο σημείο 
αυτό είναι ότι δημιουργείτε ένα αντικείμενο που αντιπροσωπεύει ένα αρχείο με το 
όνομα "myfirst.tr" και το διαβιβάζετε στο ``ns-3``. Λέτε στο ``ns-3`` να ασχοληθεί 
με τα θέματα χρόνου ζωής του δημιουργούμενου αντικειμένου και επίσης να χειριστεί 
τα προβλήματα του δημιούργησε ένας ελάχιστα γνωστός περιορισμός της C++ για 
ofstream αντικείμενα που σχετίζονται με την αντιγραφή των κατασκευαστών.

Η εξωτερική κλήση προς την ``EnableAsciiAll()``, λέει στον βοηθό ότι θέλετε να 
ενεργοποιήσετε την ιχνηλασία ASCII σε όλες τις point-to-point συσκευές της 
εξομοίωσής σας. Και θέλετε το παρεχόμενο ίχνος καταβόθρας να γράψει πληροφορίες 
σε μορφή ASCII σχετικά με την κίνηση πακέτων.

Για όσους είναι εξοικειωμένοι με το |ns2|, τα ιχνηλατημένα γεγονότα είναι ισοδύναμα 
με τα δημοφιλή σημεία ίχνους που καταγράφουν "+", "-", "d", και "r" γεγονότα.

Μπορείτε τώρα να χτίσετε το σενάριο και να το εκτελέσετε από τη γραμμή εντολών:
	
.. sourcecode:: bash

  $ ./waf --run scratch/myfirst

..
	Just as you have seen many times before, you will see some messages from Waf and then
	"'build' finished successfully" with some number of messages from 
	the running program.  

	When it ran, the program will have created a file named ``myfirst.tr``.  
	Because of the way that Waf works, the file is not created in the local 
	directory, it is created at the top-level directory of the repository by 
	default.  If you want to control where the traces are saved you can use the 
	``--cwd`` option of Waf to specify this.  We have not done so, thus we 
	need to change into the top level directory of our repo and take a look at 
	the ASCII trace file ``myfirst.tr`` in your favorite editor.

Ακριβώς όπως έχετε ήδη δει πολλές φορές, θα δείτε κάποια μηνύματα από το Waf και 
στη συνέχεια το μήνυμα  "'build' finished successfully" με κάποιο αριθμό μηνυμάτων 
από το πρόγραμμα εκτέλεσης.

Κατά την εκτέλεση το πρόγραμμα θα έχει δημιουργήσει ένα αρχείο με το όνομα 
``myfirst.tr``. Εξαιτίας του τρόπου με τον οποίο λειτουργεί το Waf, το αρχείο 
δεν έχει δημιουργηθεί στο τοπικό κατάλογο, αλλά στον προκαθορισμένο κατάλογο 
ανώτατου επιπέδου του αποθέματος. Αν θέλετε να ελέγχετε που αποθηκεύονται τα ίχνη, 
μπορείτε να χρησιμοποιήσετε την επιλογή ``--cwd`` του Waf για να το καθορίσετε. 
Εμείς δεν το κάναμε, έτσι πρέπει να αλλάξουμε κατάλογο και να πάμε στον αρχικό 
κατάλογο του αποθέματος και να ανοίξουμε το αρχείο ίχνους ASCII ``myfirst.tr`` 
με τον αγαπημένο σας επεξεργαστή κειμένου.

..
	Parsing Ascii Traces
	~~~~~~~~~~~~~~~~~~~~
	There's a lot of information there in a pretty dense form, but the first thing
	to notice is that there are a number of distinct lines in this file.  It may
	be difficult to see this clearly unless you widen your window considerably.

	Each line in the file corresponds to a *trace event*.  In this case
	we are tracing events on the *transmit queue* present in every 
	point-to-point net device in the simulation.  The transmit queue is a queue 
	through which every packet destined for a point-to-point channel must pass.
	Note that each line in the trace file begins with a lone character (has a 
	space after it).  This character will have the following meaning:

Αναλύοντας Ίχνη Ascii
~~~~~~~~~~~~~~~~~~~~~

Στο αρχείο αυτό υπάρχει ένα μεγάλο πλήθος πληροφοριών σε μια αρκετά πυκνή μορφή, 
αλλά το πρώτο πράγμα που μπορείτε να παρατηρήσετε είναι ότι υπάρχει ένας πλήθος 
από ξεχωριστές γραμμές. Ίσως είναι δύσκολο να το δείτε ξεκάθαρα αν δεν διευρύνει 
το μέγεθος του παραθύρου σας σημαντικά.

Κάθε γραμμή στο αρχείο αντιστοιχεί σε ένα *ίχνος γεγονότος*. Σε αυτήν την περίπτωση
εντοπίζουμε τα γεγονότα ιχνηλασίας στην *ουρά εκπομπής* που βρίσκεται σε κάθε
δικτυακή συσκευή point-to-point στην εξομοίωση. Η ουρά εκπομπής είναι μια ουρά
μέσω της οποίας πρέπει να περάσει κάθε πακέτο που προορίζεται για ένα κανάλι 
point-to-point. Σημειώστε ότι κάθε γραμμή στο αρχείο παρακολούθησης αρχίζει με ένα 
μοναχικό χαρακτήρα (έχει έναν κενό χαρακτήρα αμέσως μετά). Αυτός ο χαρακτήρας 
έχει την ακόλουθη έννοια:
	
..
	* ``+``: An enqueue operation occurred on the device queue;
	* ``-``: A dequeue operation occurred on the device queue;
	* ``d``: A packet was dropped, typically because the queue was full;
	* ``r``: A packet was received by the net device.

	Let's take a more detailed view of the first line in the trace file.  I'll 
	break it down into sections (indented for clarity) with a reference
	number on the left side:

* ``+``: Μια λειτουργία τοποθέτησης στην ουρά συνέβη στην ουρά συσκευής;
* ``-``: Μια λειτουργία απομάκρυνσης από την ουρά συνέβη στην ουρά συσκευής;
* ``d``: Ένα πακέτο απορρίφθηκε, συνήθως επειδή η ουρά ήταν πλήρης;
* ``r``: Ένα πακέτο παρελήφθη από την δικτυακή συσκευή.

Ας ρίξουμε μια πιο λεπτομερή ματιά στην πρώτη γραμμή του αρχείου παρακολούθησης. 
Θα την τμηματοποιήσουμε (τοποθετώντας εσοχές για λόγους σαφήνειας) με αριθμός 
αναφοράς στην αριστερή πλευρά:
	
.. sourcecode:: text
  :linenos:

  + 
  2 
  /NodeList/0/DeviceList/0/$ns3::PointToPointNetDevice/TxQueue/Enqueue 
  ns3::PppHeader (
    Point-to-Point Protocol: IP (0x0021)) 
    ns3::Ipv4Header (
      tos 0x0 ttl 64 id 0 protocol 17 offset 0 flags [none] 
      length: 1052 10.1.1.1 > 10.1.1.2)
      ns3::UdpHeader (
        length: 1032 49153 > 9) 
        Payload (size=1024)

..
	The first section of this expanded trace event (reference number 0) is the 
	operation.  We have a ``+`` character, so this corresponds to an
	*enqueue* operation on the transmit queue.  The second section (reference 1)
	is the simulation time expressed in seconds.  You may recall that we asked the 
	``UdpEchoClientApplication`` to start sending packets at two seconds.  Here
	we see confirmation that this is, indeed, happening.

	The next section of the example trace (reference 2) tell us which trace source
	originated this event (expressed in the tracing namespace).  You can think
	of the tracing namespace somewhat like you would a filesystem namespace.  The 
	root of the namespace is the ``NodeList``.  This corresponds to a container
	managed in the |ns3| core code that contains all of the nodes that are
	created in a script.  Just as a filesystem may have directories under the 
	root, we may have node numbers in the ``NodeList``.  The string 
	``/NodeList/0`` therefore refers to the zeroth node in the ``NodeList``
	which we typically think of as "node 0".  In each node there is a list of 
	devices that have been installed.  This list appears next in the namespace.
	You can see that this trace event comes from ``DeviceList/0`` which is the 
	zeroth device installed in the node. 

Το πρώτο τμήμα αυτού του διευρυμένου γεγονότος ίχνους (αριθμός αναφοράς 0) είναι η
λειτουργία. Έχουμε ένα χαρακτήρα ``+``, οπότε αυτό αντιστοιχεί σε μια λειτουργία
*τοποθέτησης στην ουρά* στην ουρά εκπομπής. Το δεύτερο τμήμα (αναφορά 1) είναι ο 
χρόνος εξομοίωσης που εκφράζεται σε δευτερόλεπτα. Ίσως να θυμάστε ότι ζητήσαμε 
από το ``UdpEchoClientApplication`` να ξεκινήσετε την αποστολή πακέτων στα δύο 
δευτερόλεπτα. Εδώ βλέπουμε την επιβεβαίωση ότι αυτό πράγματι συμβαίνει.

Το επόμενο τμήμα του ίχνους του παραδείγματος (αναφορά 2) μας δείχνει από ποια πηγή 
ίχνους προήλθε αυτό το γεγονός (εκφράζεται στο χώρο ονομάτων εντοπισμού). Μπορείτε 
να σκεφτείτε ότι ο χώρος ονομάτων του εντοπισμού είναι παρόμοιος με τον χώρο ονομάτων 
αρχείων. Η ρίζα του χώρου ονομάτων είναι η ``NodeList``. Αυτό αντιστοιχεί σε ένα δοχείο
διαχειρίζεται το | NS3 | κωδικός πυρήνα που περιέχει το σύνολο των κόμβων που είναι
δημιουργήθηκε σε ένα σενάριο. Ακριβώς όπως ένα σύστημα αρχείων μπορεί να έχει καταλόγους κάτω από το
ρίζα, μπορεί να έχουμε τους αριθμούς κόμβου στο ``NodeList``. Το κορδόνι
`` / NodeList / 0`` αναφέρεται, επομένως, στον κόμβο μηδενικής στην ``NodeList``
οποία συνήθως σκεφτόμαστε ως «κόμβος 0". Σε κάθε κόμβο υπάρχει μια λίστα
συσκευές που έχουν εγκατασταθεί. Αυτή η λίστα εμφανίζεται δίπλα στο χώρο ονομάτων.
Μπορείτε να δείτε ότι αυτό το γεγονός ίχνος προέρχεται από ``DeviceList/0`` η οποία είναι η
συσκευή μηδενικής εγκατεστημένο στον κόμβο.

..
	The next string, ``$ns3::PointToPointNetDevice`` tells you what kind of 
	device is in the zeroth position of the device list for node zero.
	Recall that the operation ``+`` found at reference 00 meant that an enqueue 
	operation happened on the transmit queue of the device.  This is reflected in 
	the final segments of the "trace path" which are ``TxQueue/Enqueue``.

	The remaining sections in the trace should be fairly intuitive.  References 3-4
	indicate that the packet is encapsulated in the point-to-point protocol.  
	References 5-7 show that the packet has an IP version four header and has
	originated from IP address 10.1.1.1 and is destined for 10.1.1.2.  References
	8-9 show that this packet has a UDP header and, finally, reference 10 shows
	that the payload is the expected 1024 bytes.

	The next line in the trace file shows the same packet being dequeued from the
	transmit queue on the same node. 

	The Third line in the trace file shows the packet being received by the net
	device on the node with the echo server. I have reproduced that event below.

Το επόμενο αλφαριθμητικό, ``$ns3::PointToPointNetDevice`` σας λέει τι είδους
συσκευή είναι στη μηδενική θέση στη λίστα συσκευών για τον κόμβο μηδέν.
Θυμηθείτε ότι η λειτουργία ``+`` στην αναφορά 00 σημαίνει ότι μια λειτουργία 
τοποθέτησης στην ουρά συνέβη στην ουρά μεταδόσεως της συσκευής. Αυτό αντικατοπτρίζεται 
στα τελικά τμήματα της "διαδρομής ίχνους" τα οποίο είναι ``TxQueue/Enqueue``.

Τα υπόλοιπα τμήματα στο ίχνος πρέπει να είναι αρκετά έξυπνα. Οι αναφορές 3-4
υποδεικνύουν ότι το πακέτο είναι εμφωλιασμένο στο πρωτόκολλο point-to-point.
Οι αναφορές 5-7 δείχνουν ότι το πακέτο έχει μια επικεφαλίδα IPv4 και προήλθε από 
τη διεύθυνση IP 10.1.1.1 και έχει προορισμό την 10.1.1.2. Οι αναφορές 8-9 δείχνουν 
ότι αυτό το πακέτο έχει μια επικεφαλίδα UDP και, τέλος, η αναφορά 10 δείχνει
ότι το ωφέλιμο φορτίο είναι τα αναμενόμενα 1024 bytes.

Η επόμενη γραμμή στο αρχείο ίχνος δείχνει το ίδιο πακέτο που απομακρύνεται από την
ουρά μετάδοσης στον ίδιο κόμβο.

Η τρίτη γραμμή στο αρχείο ίχνος δείχνει το πακέτο που λήφθηκε από τη δικτυακή συσκευή
μέσω της αντήχησης του εξυπηρετητή. Αναπαράγουμε αυτό το συμβάν παρακάτω.

.. sourcecode:: text
  :linenos:

  r 
  2.25732 
  /NodeList/1/DeviceList/0/$ns3::PointToPointNetDevice/MacRx 
    ns3::Ipv4Header (
      tos 0x0 ttl 64 id 0 protocol 17 offset 0 flags [none]
      length: 1052 10.1.1.1 > 10.1.1.2)
      ns3::UdpHeader (
        length: 1032 49153 > 9) 
        Payload (size=1024)


..
	Notice that the trace operation is now ``r`` and the simulation time has
	increased to 2.25732 seconds.  If you have been following the tutorial steps
	closely this means that you have left the ``DataRate`` of the net devices	
	and the channel ``Delay`` set to their default values.  This time should 
	be familiar as you have seen it before in a previous section.

	The trace source namespace entry (reference 02) has changed to reflect that
	this event is coming from node 1 (``/NodeList/1``) and the packet reception
	trace source (``/MacRx``).  It should be quite easy for you to follow the 
	progress of the packet through the topology by looking at the rest of the 
	traces in the file.

Παρατηρήστε ότι η λειτουργία ανίχνευσης είναι πλέον ``r`` και ο χρόνος εξομοίωσης 
έχει αυξηθεί σε 2.25732 δευτερόλεπτα. Αν έχετε ακολουθήσει τα βήματα του οδηγού 
προσεκτικά, αυτό σημαίνει ότι έχετε αφήσει το ``DataRate`` των δικτυακών συσκευών
και το κανάλι ``Delay`` στις προεπιλεγμένες τιμές τους. Αυτή τη φορά θα πρέπει να
είστε εξοικειωμένοι μια που το έχετε ξαναδεί σε προηγούμενη ενότητα.

Η είσοδος χώρος ονομάτων της πηγής ίχνους (αναφορά 02) έχει αλλάξει για να 
επισημάνει ότι το γεγονός έρχεται από τον κόμβο 1 (``/NodeList/1``) και η λήψη πακέτου
της πηγή ίχνους (``/MacRx``). Θα πρέπει να είναι αρκετά εύκολο για σας να ακολουθήσετε
την πρόοδο του πακέτου μέσω της τοπολογίας κοιτάζοντας τα υπόλοιπα ίχνη του αρχείου.

..
	PCAP Tracing
	++++++++++++
	The |ns3| device helpers can also be used to create trace files in the
	``.pcap`` format.  The acronym pcap (usually written in lower case) stands
	for packet capture, and is actually an API that includes the 
	definition of a ``.pcap`` file format.  The most popular program that can
	read and display this format is Wireshark (formerly called Ethereal).
	However, there are many traffic trace analyzers that use this packet format.
	We encourage users to exploit the many tools available for analyzing pcap
	traces.  In this tutorial, we concentrate on viewing pcap traces with tcpdump.

	The code used to enable pcap tracing is a one-liner.  

Οι βοηθοί συσκευών |ns3| μπορούν επίσης να χρησιμοποιηθούν για τη δημιουργία 
αρχείων ίχνους σε μορφή ``.pcap``. Το αρκτικόλεξο pcap αντιστοιχεί στη σύλληψη 
πακέτων (packet capture) και συνήθως γράφεται με μικρά γράμματα. Είναι στην 
πραγματικότητα μια διεπαφή προγράμματος που περιλαμβάνει τον ορισμό του είδους 
αρχείου ``.pcap``. Το πιο δημοφιλές πρόγραμμα που μπορεί να εμφανίσει αυτό το είδος 
αρχείου είναι το Wireshark (παλαιότερα ονομαζόταν Ethereal). Ωστόσο, υπάρχουν πολλοί 
αναλυτές ίχνους κίνησης που χρησιμοποιούν αυτή τη μορφή πακέτων. Ενθαρρύνουμε τους 
χρήστες να εκμεταλλευτούν τα πολλά διαθέσιμα εργαλεία για την ανάλυση ιχνών pcap. 
Σε αυτόν τον οδηγό, θα επικεντρωθούμε στην προβολή ιχνών pcap με το tcpdump.

Ο κωδικός που χρησιμοποιούμε για να ενεργοποιήσουμε την ιχνηλασία pcap είναι 
μιας γραμμής.
	
::

  pointToPoint.EnablePcapAll ("myfirst");

..
	Go ahead and insert this line of code after the ASCII tracing code we just 
	added to ``scratch/myfirst.cc``.  Notice that we only passed the string
	"myfirst," and not "myfirst.pcap" or something similar.  This is because the 
	parameter is a prefix, not a complete file name.  The helper will actually 
	create a trace file for every point-to-point device in the simulation.  The 
	file names will be built using the prefix, the node number, the device number
	and a ".pcap" suffix.

	In our example script, we will eventually see files named "myfirst-0-0.pcap" 
	and "myfirst-1-0.pcap" which are the pcap traces for node 0-device 0 and 
	node 1-device 0, respectively.

	Once you have added the line of code to enable pcap tracing, you can run the
	script in the usual way:

Εισάγετε αυτή τη γραμμή του κώδικα μετά τον κωδικό ιχνηλασίας ASCII που μόλις 
προσθέσατε στο ``scratch/myfirst.cc``. Παρατηρήστε ότι έχουμε περάσει μόνο το 
αλφαριθμητικό "myfirst," και όχι "myfirst.pcap" ή κάτι παρόμοιο. Αυτό συμβαίνει 
επειδή η παράμετρος είναι ένα πρόθεμα, δεν είναι ένα πλήρες όνομα του αρχείου. 
Ο βοηθός στην ουσία θα δημιουργήσει ένα αρχείο ίχνους για κάθε συσκευή point-to-point 
στην εξομοίωση. Τα ονόματα των αρχείων θα χτιστούν χρησιμοποιώντας το πρόθεμα, 
τον αριθμό κόμβου, τον αριθμό της συσκευής και μια κατάληξη ".pcap".

Στο σενάριο του παραδείγματός μας, θα δούμε τελικά αρχεία με όνομα "myfirst-0-0.pcap"
και "myfirst-1-0.pcap" που είναι τα ίχνη pcap για τον κόμβο 0-συσκευή 0 και
κόμβο 1-συσκευή 0, αντίστοιχα.

Μόλις έχετε προσθέσει τη γραμμή του κώδικα για να ενεργοποιήσετε την ιχνηλασία 
pcap, μπορείτε να εκτελέσετε το σενάριο με το συνήθη τρόπο:

.. sourcecode:: bash

  $ ./waf --run scratch/myfirst

..
	If you look at the top level directory of your distribution, you should now
	see three log files:  ``myfirst.tr`` is the ASCII trace file we have 
	previously examined.  ``myfirst-0-0.pcap`` and ``myfirst-1-0.pcap``
	are the new pcap files we just generated.  

	Reading output with tcpdump
	~~~~~~~~~~~~~~~~~~~~~~~~~~~
	The easiest thing to do at this point will be to use ``tcpdump`` to look
	at the ``pcap`` files.  

Αν κοιτάξετε στον κατάλογο κορυφής της διανομής σας, θα πρέπει τώρα να βλέπετε
τρία αρχεία καταγραφής: ``myfirst.tr`` είναι το αρχείο ίχνους ASCII που έχουμε
εξετάσει προηγουμένως. Τα ``myfirst-0-0.pcap`` και ``myfirst-1-0.pcap`` είναι τα 
νέα αρχεία pcap που μόλις δημιουργήσαμε.

Ανάγνωση εξόδου με tcpdump
~~~~~~~~~~~~~~~~~~~~~~~~~~
Το πιο εύκολο βήμα που μπορούμε κάνουμε σε αυτό το σημείο θα είναι να χρησιμοποιήσουμε 
το ``tcpdump`` να δούμε τα ``pcap`` αρχεία.

.. sourcecode:: bash

  $ tcpdump -nn -tt -r myfirst-0-0.pcap
  reading from file myfirst-0-0.pcap, link-type PPP (PPP)
  2.000000 IP 10.1.1.1.49153 > 10.1.1.2.9: UDP, length 1024
  2.514648 IP 10.1.1.2.9 > 10.1.1.1.49153: UDP, length 1024

  tcpdump -nn -tt -r myfirst-1-0.pcap
  reading from file myfirst-1-0.pcap, link-type PPP (PPP)
  2.257324 IP 10.1.1.1.49153 > 10.1.1.2.9: UDP, length 1024
  2.257324 IP 10.1.1.2.9 > 10.1.1.1.49153: UDP, length 1024

..
	You can see in the dump of ``myfirst-0-0.pcap`` (the client device) that the 
	echo packet is sent at 2 seconds into the simulation.  If you look at the
	second dump (``myfirst-1-0.pcap``) you can see that packet being received
	at 2.257324 seconds.  You see the packet being echoed back at 2.257324 seconds
	in the second dump, and finally, you see the packet being received back at 
	the client in the first dump at 2.514648 seconds.

	Reading output with Wireshark
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	If you are unfamiliar with Wireshark, there is a web site available from which
	you can download programs and documentation:  http://www.wireshark.org/.

	Wireshark is a graphical user interface which can be used for displaying these
	trace files.  If you have Wireshark available, you can open each of the trace
	files and display the contents as if you had captured the packets using a
	*packet sniffer*.

Μπορείτε να δείτε στο dump του αρχείου ``myfirst-0-0.pcap`` (η συσκευή του πελάτη) 
ότι η το πακέτο αντανάκλασης στέλνεται στα 2 δευτερόλεπτα στην εξομοίωση. Αν κοιτάξετε
το δεύτερο dump (``myfirst-1-0.pcap``) μπορείτε να δείτε ότι το πακέτο λαμβάνεται
σε 2.257324 δευτερόλεπτα. Μπορείτε να δείτε το πακέτο που αντανακλάται πίσω σε 2.257324 
δευτερόλεπτα στο δεύτερο dump, και, τέλος, μπορείτε να δείτε το πακέτο που παραλαμβάνεται 
πίσω στον πελάτη στο πρώτο dump σε 2.514648 δευτερόλεπτα.

Ανάγνωση εξόδου με το Wireshark
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Εάν δεν είστε εξοικειωμένοι με το Wireshark, υπάρχει μια ιστοσελίδα από την οποία 
μπορείτε να κατεβάσετε τα προγράμματα και την τεκμηρίωση: http://www.wireshark.org/.

Το Wireshark είναι ένα γραφικό περιβάλλον χρήστη, το οποίο μπορεί να χρησιμοποιηθεί 
για την εμφάνιση αυτών των αρχείων ίχνους. Εάν έχετε διαθέσιμο το Wireshark, μπορείτε 
να ανοίξετε κάθε αρχείο ίχνους και να εμφανίσετε τα περιεχόμενά του σαν να είχαν συλληφθεί 
τα πακέτα χρησιμοποιώντας έναν οσφρηστή πακέτων (*packet sniffer*).
