# Python-Project
Shows how Othello works.
import check

## A Board is a (listof (listof Str))
## Requires:
##   The length of the outer list is 8.
##   The length of each inner list is 8.
##   Each string is '', 'B', or 'W'.


##Two sample boards to help with testing.
##You may change these as needed.
##The second is different from the above.

board = [[ '',  '',  '',  '',  '',  '',  '',  ''],
         [ '',  '',  '',  '',  '',  '',  '',  ''],
         [ '',  '', 'B', 'B', 'B',  '',  '',  ''],
         [ '', 'W', 'B', 'W', 'W',  '', 'B',  ''],
         [ '', 'W', 'B', 'W', 'W', 'W', 'W',  ''],
         [ '',  '', 'W', 'W', 'W',  '',  '',  ''],
         [ '',  '',  '', 'W',  '',  '',  '',  ''],
         [ '',  '',  '',  '',  '',  '',  '',  '']]
         
board2 = [[ 'B', 'B', 'B', 'B', 'B', 'B', 'B', 'B'],
          [ 'B', 'W', 'B', 'W', 'B', 'B', 'W', 'B'],
          [ 'B', 'B', 'W', 'W', 'B', 'B', 'B', 'B'],
          [ 'B', 'W', 'W', 'W', 'W', 'W', 'W', 'W'],
          [ 'W', 'W', 'W', 'W', 'W', 'W', 'B', 'B'],
          [ 'B', 'B', 'W', 'W', 'W', 'B', 'B', 'W'],
          [ 'B', 'B', 'W', 'B', 'B', 'B', 'W', 'W'],
          [ 'B', 'B', 'W', 'B', 'B', 'B', 'W', 'W']]


def is_another_colour_surrounded(board, turn, row, col):
  '''
  Consumes a board of type Board,  turn which is one of 'B' or 'W' 
  and two parameters row and col, natural numbers between 0 and 7 inclusive,
  which represents the square located on the Board where we are trying to put a piece.
  Returns a nested list of the natural number representing the position of another-color
  discs, and False if there's no another-color disc.
  
  is_another_colour_surrounded: Board Str Nat Nat -> (Anyof False (listof (list Nat Nat)))
  Requires:
  For Board: The length of the outer list is 8.
             The length of each inner list is 8.
             Each string is '', 'B', or 'W'.
  turn has to be either 'W' or 'B'
  row and col are both between 0 and 7 inclusive.
  '''
  if board[row][col] != '':
    return False
  L = []
  for n in range(len(board)):
    if (n == row - 1) or (n == row) or (n == row + 1):
      for m in range(len(board[n])):
        if (m == col - 1) or (m == col) or (m == col + 1):
          if turn == 'B':
            if board[n][m] == 'W':
              L = L + [[n, m]]
          if turn == 'W':
            if board[n][m] == 'B':
              L = L + [[n, m]]
  if L == []:
    return False
  else:
    return L

def test_eight_directions(board, turn, row, col, ano_colour):
  '''
  Consumes a board of type Board, turn which is one of 'B' or 'W' 
  and two parameters row and col, natural numbers between 0 and 7 inclusive,
  which represents the square located in the Board where we are trying to
  put a piece and a listof list of Nat that represents the position of the 
  another colour disc. Return True if the given position is valid and False otherwise.
  
  test_eight_directions: Board Str Nat Nat (listof (list Nat Nat)) -> Bool
  Requires:
  For Board: The length of the outer list is 8.
             The length of each inner list is 8.
             Each string is '', 'B', or 'W'.
  turn has to be either 'W' or 'B'
  row and col are both between 0 and 7 inclusive.
  '''
  m = ano_colour[0]
  n = ano_colour[1]
  a = ano_colour[0] - row
  b = ano_colour[1] - col
  while n <= 7 and n >= 0 and m >= 0 and m <= 7:
    if board[m][n] == turn:
      return True
    else:
      n = n + b
      m = m + a
  return False

def othello(board, turn, row, col):
  '''
  Consumes a board of type Board, turn which is one of 'B' or 'W' and 
  two parameters row and col, natural numbers between 0 and 7 inclusive, 
  which represents the square located in the Board where we are trying 
  to put a piece and returns True if the piece of color turn can be played 
  at the given location as a valid move and False otherwise.
  
  othello: Board Str Nat Nat -> Bool
  Requires:
  For Board: The length of the outer list is 8.
             The length of each inner list is 8.
             Each string is '', 'B', or 'W'.
  turn has to be either 'W' or 'B'
  row and col are both between 0 and 7 inclusive.
  
  Examples:
  othello(board, 'B', 3, 0) => True
  othello(board, 'W', 3, 0) => False
  othello(board2, 'W', 1, 2) => False
  '''
  L = is_another_colour_surrounded(board, turn, row, col)
  if L == False:
    return False
  else:
    for n in range(len(L)):
      if test_eight_directions(board, turn, row, col, L[n]):
        return True
    return False

##Examples as Tests:
check.expect("Example1", othello(board, 'B', 3, 0), True)
check.expect("Example2", othello(board, 'W', 3, 0), False)

##Tests:
check.expect("The place is taken1", othello(board2, 'W', 4, 2), False)
check.expect("The place is taken2", othello(board, 'B', 4, 4), False)
check.expect("Black turn", othello(board, 'B', 5, 5), True)
check.expect("White turn", othello(board, 'W', 1, 2), True)

 
board3 = [[ '', '', '', '', '', '', 'B', ''],
          [ '', '', 'B', '', '', 'B', '', ''],
          [ '', '', 'W', 'W', 'W', 'B', 'B', ''],
          [ '', '', '', 'W', 'B', 'B', '', ''],
          [ '', '', 'B', 'B', 'W', 'B', 'B', ''],
          [ '', '', 'W', '', '', 'B', '', ''],
          [ '', '', '', '', '', 'W', '', ''],
          [ '', '', '', '', '', '', '', '']] 
          
check.expect("My test1", othello(board3, 'W', 6, 6), True)
check.expect("My test2", othello(board3, 'W', 1, 3), False)
check.expect("My test3", othello(board3, 'B', 3, 2), True)
check.expect("My test4", othello(board3, 'B', 3, 6), False)
