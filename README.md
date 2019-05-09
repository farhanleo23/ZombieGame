# ZombieGame


%Movie Data
movie_data(avengers, pecsCC, 10, 0).
movie_data(avengers, budapestCC, 20, 0).
movie_data(avengers, gyorCC, 20, 0).
movie_data(avengers, szegedCC, 10, 0).
movie_data(it, pecsCC, 10, 0).
movie_data(it, budapestCC, 20, 0).
movie_data(it, szegedCC, 15, 0).

%All the movies available
movie([avengers, it]).

:- dynamic movie_data/4.

%Replace existing fact
replace_existing_fact(OldFact, NewFact) :- call(OldFact), retract(OldFact), assert(NewFact).

header :- writeln("------------ CINEMA CITY -----------"), writeln("Pick a movie: ").

pickTheatre(Theatre) :- writeln("Pick a theatre"), read(Theatre).

pickSeats(Seats) :- writeln("How many seats do you need?"), read(Seats).

% Start the ticket buyer
start :- header, movie(Movies), writeln(Movies), read(Selected_Movie), findall(Theatres, movie_data(Selected_Movie, Theatres, _, _), ListTheatres), writeln(ListTheatres), pickTheatre(Theatre), pickSeats(Seats), Cost is Seats * 1000, writef("You owe %w Ft.\n", [Cost]), movie_data(Selected_Movie, Theatre,X, Y), Z is Y + Seats, replace_existing_fact(movie_data(Selected_Movie, Theatre, X, Y), movie_data(Selected_Movie, Theatre, X, Z)), writeln("Would you like to buy more tickets?(y or n)"), read(Decision), (Decision == y -> start; Decision == n -> writeln("Thank you for shopping with us"); write('\nInvalid Input!!!\n'),fail).

%Find which theatre recieved most guests
theatreGuests(Theatre, G) :- bagof(Seats, Movie^Capacity^movie_data(Movie, Theatre, Capacity, Seats), SL), sum(SL, G).

%wHCIH movie was seen the most
movieGuests(Movie, G) :- bagof(Seats, Theatre^Capacity^movie_data(Movie, Theatre, Capacity, Seats), SL), sum(SL, G).

%hoW MANY guests visited all together
allGuests(G) :- bagof(Seats, Theatre^Movie^Capacity^movie_data(Movie, Theatre, Capacity, Seats), SL), sum(SL, G).

%Gives Income of each theatre
theatreIncome(Theatre, G) :- bagof(Seats, Movie^Capacity^movie_data(Movie, Theatre, Capacity, Seats), SL), sum(SL, X), G is 1000 * X, assert(income(Theatre, G)).

% Sorts income of theatre in decreasing order.
sortTheatreIncome(Output) :- findall([X,Y],income(X,Y),List),
sort(2, @>, List, OrderedList),
Output = OrderedList.


sum([], 0).
sum([H|T], S) :- sum(T, S1), S is S1 + H.


