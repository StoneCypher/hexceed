# hexceed
erlang module implementing the rules to a silly little boardgame in my head





## Rules
The rules of Hexceed, a game for two players.  People moving quickly should expect a game to take less than five minutes.

### The Board
`Hexceed` is played on a hex board of diameter 5, with the center cell removed:

      * * *
     * * * *
    * *   * *
     * * * *
      * * *

### Pieces
To fill the eighteen cells of this board, eighteen pieces are provided, shared by the two players.  

To make 18 pieces, each piece differs in three ways, with ranges 3,3,2.  We will call these "qualities."  The set of pieces is every combination of the three qualities.  

The reference implementation uses three colors [`R`,`G`,`B`] for the piece, three symbols [`X`,`O`,`-`] for the foreground, and two colors [`K`/`W`] of the symbol.  (`K` is the printer's color for `Black`, because `B` is already `Blue`.  That's why it's `CMYK`.)

<image src="pieces/rxw.png" width="48"/>
<image src="pieces/row.png" width="48"/>
<image src="pieces/rdw.png" width="48"/>
<image src="pieces/rxk.png" width="48"/>
<image src="pieces/rok.png" width="48"/>
<image src="pieces/rdk.png" width="48"/><br/>
<image src="pieces/gxw.png" width="48"/>
<image src="pieces/gow.png" width="48"/>
<image src="pieces/gdw.png" width="48"/>
<image src="pieces/gxk.png" width="48"/>
<image src="pieces/gok.png" width="48"/>
<image src="pieces/gdk.png" width="48"/><br/>
<image src="pieces/bxw.png" width="48"/>
<image src="pieces/bow.png" width="48"/>
<image src="pieces/bdw.png" width="48"/>
<image src="pieces/bxk.png" width="48"/>
<image src="pieces/bok.png" width="48"/>
<image src="pieces/bdk.png" width="48"/>

(The three representations of difference are unimportant, provided they're easy to read, and different sets may approach this different ways; another might use shape, texture, and base material.  This document will proceed using the reference implementation's choices, for brevity's sake.)

### Turns
The players "first" and "second" take alternating turns, starting with the first player choosing a piece, and the second player placing it; then the second player choosing a piece, and the first player placing it, until the board is filled.

To place a piece, the player chooses one of the empty cells of the board, and places their piece in that cell.  Pieces are not moved until the game is over.  When a player places their piece, count and add to their score the points from that placement, then choose a piece for the opponent to play, and move on.

### Scoring a placement
For any placement, if a line of three or small triangle of a single quality goes through the placement, score a point for each.

A placement can be a member of any number of triangles or lines of a single quality.  

Let's label the board cells, to make explanations easier.

      A B C
     D E F G
    H I   J K
     L M N O
      P Q R

Suppose a white-plus-on-red is in B; a white-plus-on-green is in F; a white-x-on-blue is in I; and a white-o-on-blue is in D.

Someone plays white-plus-on-blue in E.

This means that the triangle whites in BEF, plus in BEF, blue in DIE, and the lines white in DEF and white in BEI have all been made, for five total points.  Notice that BEF scores twice - once for all white, and once for all plus.

Depending on what's in A, L, or G, it's possible that this player could receive other points for their play at E (but the example is already too complicated.)

### Lines and triangles
A closer definition of lines and triangles, just in case.

A line of three is three cells in a straight row, skipping over no cells, in the same direction.  So, pieces at BFJ are a line, but BFO are not (no gaps like J) and BFG are not (not a straight line.)

A small triangle is three cells who are all immediate neighbors.  So, BEF is a small triangle, and BFC is a small triangle, but FMO is not (too large) and BFG is not (B and G are not immediate neighbors.)  Because of the wording of the rule, the triangles will all be tiny unit equilateral triangles.

The order of cells in a line or triangle does not matter.  BEF and FBE describe the same triangle, and are not scored separately.  Same goes for BEI and IEB: same line.

This means that a board contains 18 lines and 18 triangles.  Six horizontal lines ABC DEF EFG LMN MNO PQR, six bend diagonals HDA LIE IEB QNJ NJG ROK, six sinister diagonals HLP DIM IMQ BFJ FJO CGK; nine down-pointing triangles AEB BFC DIE FJG HLI JOK LPM MQN NRO, and nine up-pointing triangles ADE EBF FCG HDI JGK LIM NJO PMQ QNR.
