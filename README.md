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

The reference implementation uses three colors [`R`,`G`,`B`] for the piece, three symbols [`X`,`O`,`=`] for the foreground, and two colors [`K`/`W`] of the symbol.  (`K` is the printer's color for `Black`, because `B` is already `Blue`.  That's why it's `CMYK`.)

(The three representations of difference are unimportant, provided they're easy to read, and different sets may approach this different ways; another might use shape, texture, and base material.  

### Turns
The players "first" and "second" take alternating turns, starting with the first player choosing a piece, and the second player placing it; then the second player choosing a piece, and the first player placing it, until the board is filled.

To place a piece, the player chooses one of the empty cells of the board, and places their piece in that cell.  Pieces are not moved until the game is over.  When a player places their piece, count and add to their score the points from that placement, then choose a piece for the opponent to play, and move on.

### Scoring a placement
For any placement, if a line of three or a small triangle goes through the placement, score a point for each.  

      * * *
     * * * *
    * *   * *
     * * * *
      * * *

A line of three is three cells in a row, skipping over no cells, in the same direction.
