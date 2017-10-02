# CCG
A simple (yet efficient) Prolog implemantation of the Combinatory Categorial Grammar (CCG) formalism.

As for now, the program can only be interpreted using the interpreter specified below. I'll try to translate it one day so it can be interpreted by SWI-Prolog...

This version is able to handle forward and backward application, forward and backward composition and forward type-raising (which is enough to parse sentences written in French)

# Usage
If you try to parse a sentence without supplying the appropriate vocabulary, the program will be unable to its job. Make sure you add the vocabulary you want to parse in the *cat* clause :

```prolog
cat("que", "S/S").
cat("que", "NP\NP/S/NP").
cat("qui", "NP\NP/S\NP").
```

Then, load the program : "Insérer>Insérer la fenêtre" and you are ready to go :) ! Write your sentence in the following clause *demarrage("Your sentence").*, press 'Enter' and enjoy!

Sample output:

```
?- demarrage("toto et paul attrapent et mangent mickey et garfield").
-------------------
		toto et paul attrapent et mangent mickey et garfield


["toto","NP","et","X\X/X","paul","NP","attrapent","S\NP/NP","et","X\X/X","mangent","S\NP/NP","mickey","NP","et","X\X/X","garfield","NP"]



	[toto]=>NP 	[et]=>X\X/X 	[paul]=>NP
	-------------------->*
		NP

	[attrapent]=>S\NP/NP 	[et]=>X\X/X 	[mangent]=>S\NP/NP
	-------------------->*
		S\NP/NP

	["toto","et","paul"]=>NP 		["attrapent","et","mangent"]=>S\NP/NP
	-------------------->T
		S/S\NP

	["toto","et","paul"]=>S/S\NP 	["attrapent","et","mangent"]=>S\NP/NP
	-------------------->B
		S/NP

	[mickey]=>NP 	[et]=>X\X/X 	[garfield]=>NP
	-------------------->*
		NP

	[["toto","et","paul"] ["attrapent","et","mangent"] ]=>S/NP 	["mickey","et","garfield"]=>NP
	-------------------->
		S

	[[["toto","et","paul"],["attrapent","et","mangent"]],["mickey","et","garfield"]] => "S"

	Réussite !




	[toto]=>NP 	[et]=>X\X/X 	[paul]=>NP
	-------------------->*
		NP

	[attrapent]=>S\NP/NP 	[et]=>X\X/X 	[mangent]=>S\NP/NP
	-------------------->*
		S\NP/NP

	["toto","et","paul"]=>NP 		["attrapent","et","mangent"]=>S\NP/NP
	-------------------->T
		S/S\NP

	["toto","et","paul"]=>S/S\NP 	["attrapent","et","mangent"]=>S\NP/NP
	-------------------->B
		S/NP

	[mickey]=>NP 	[et]=>X\X/X 	[garfield]=>NP
	-------------------->*
		NP

	[["toto","et","paul"] ["attrapent","et","mangent"] ]=>S/NP 	["mickey","et","garfield"]=>NP
	-------------------->
		S

	[[["toto","et","paul"],["attrapent","et","mangent"]],["mickey","et","garfield"]] => "S"

	Réussite !




	[toto]=>NP 	[et]=>X\X/X 	[paul]=>NP
	-------------------->*
		NP

	[attrapent]=>S\NP/NP 	[et]=>X\X/X 	[mangent]=>S\NP/NP
	-------------------->*
		S\NP/NP

	[mickey]=>NP 	[et]=>X\X/X 	[garfield]=>NP
	-------------------->*
		NP

	["attrapent","et","mangent"]=>S\NP/NP 	["mickey","et","garfield"]=>NP
	-------------------->
		S\NP

	["toto","et","paul"]=>NP 	[["attrapent","et","mangent"] ["mickey","et","garfield"] ]=>S\NP
	--------------------<
		S

	[["toto","et","paul"],[["attrapent","et","mangent"],["mickey","et","garfield"]]] => "S"

	Réussite !

	Il y a 2 solutions

[["toto","et","paul"],[["attrapent","et","mangent"],["mickey","et","garfield"]]]
[[["toto","et","paul"],["attrapent","et","mangent"]],["mickey","et","garfield"]]


["toto","NP","et","X\X/X","paul","NP","attrapent","S\NP/NP","et","X\X/X","mangent","S\NP","mickey","NP","et","X\X/X","garfield","NP"]
	Il n'y a aucune solution !


16 millisecondes pour 9 tokens{}
```

```
?- demarrage("garfield attrape et mange mickey").
-------------------
		garfield attrape et mange mickey


["garfield","NP","attrape","S\NP/NP","et","X\X/X","mange","S\NP/NP","mickey","NP"]



	[attrape]=>S\NP/NP 	[et]=>X\X/X 	[mange]=>S\NP/NP
	-------------------->*
		S\NP/NP

	[garfield]=>NP 		["attrape","et","mange"]=>S\NP/NP
	-------------------->T
		S/S\NP

	[garfield]=>S/S\NP 	["attrape","et","mange"]=>S\NP/NP
	-------------------->B
		S/NP

	["garfield",["attrape","et","mange"] ]=>S/NP 	[mickey]=>NP
	-------------------->
		S

	[["garfield",["attrape","et","mange"]],"mickey"] => "S"

	Réussite !




	[attrape]=>S\NP/NP 	[et]=>X\X/X 	[mange]=>S\NP/NP
	-------------------->*
		S\NP/NP

	[garfield]=>NP 		["attrape","et","mange"]=>S\NP/NP
	-------------------->T
		S/S\NP

	[garfield]=>S/S\NP 	["attrape","et","mange"]=>S\NP/NP
	-------------------->B
		S/NP

	["garfield",["attrape","et","mange"] ]=>S/NP 	[mickey]=>NP
	-------------------->
		S

	[["garfield",["attrape","et","mange"]],"mickey"] => "S"

	Réussite !




	[attrape]=>S\NP/NP 	[et]=>X\X/X 	[mange]=>S\NP/NP
	-------------------->*
		S\NP/NP

	["attrape","et","mange"]=>S\NP/NP 	[mickey]=>NP
	-------------------->
		S\NP

	[garfield]=>NP 	[["attrape","et","mange"] "mickey"]=>S\NP
	--------------------<
		S

	["garfield",[["attrape","et","mange"],"mickey"]] => "S"

	Réussite !

	Il y a 2 solutions

["garfield",[["attrape","et","mange"],"mickey"]]
[["garfield",["attrape","et","mange"]],"mickey"]


["garfield","NP","attrape","S\NP/NP","et","X\X/X","mange","S\NP","mickey","NP"]
	Il n'y a aucune solution !


0 millisecondes pour 5 tokens{}
```

# PrologII+
This program was - unfortunately - written in an old version of Prolog, called PrologII+, using the Edinburgh syntax. PrologII+ binary can be found [here](http://www.lif-sud.univ-mrs.fr/~paulsab/PrologII+/PrologII+_Windows/PrologII+.zip) (Windows only). 
Once the installation is complete, you need to specify you'll be using the Edinburgh syntax. To do so, right-click on the binary and add the following parameters "-E -c 1000000"
