Swift Data :-

1) For open Emojis in text press ctrl + command + Space
2) For Select Color programatically ==> Color Literal 

==> // Turnery Operator in Swift:-
SYNTAX :- value = condition ? valueIfTrue : valueIfFalse
EX :- cell.acessoryType = item.done==True ? .checkmark : .none
DETAIL CONDITION :-
     if item.done == true {
         cell.accessoryType = .checkmark
     } else {
          cell.accessoryType = .none
     }
     
==> // Selected Not Selected in Single Line :-

 itamArry[indexpath.row].done = !itemArry[indexpath.row].done
DETAIL CONDITION :- 
    if itemArry[indexpath.row].done == false {
        itemArry[indexpath.row].done = true
    } else {
        itemArry[indexpath.row].done = false
    }
