# blackjackgame
import random
suits = ('Hearts', 'Diamonds', 'Spades', 'Clubs')
ranks = ('Two', 'Three', 'Four', 'Five', 'Six', 'Seven', 'Eight', 'Nine', 'Ten', 'Jack', 'Queen', 'King', 'Ace')
values = {'Two':2, 'Three':3, 'Four':4, 'Five':5, 'Six':6, 'Seven':7, 'Eight':8, 'Nine':9, 'Ten':10, 'Jack':10,
         'Queen':10, 'King':10, 'Ace':11}
playing = 
class Card():
    def __init__(self,suit,rank):
        self.suit = suit
        self.rank = rank
    def __str__(self):
        return self.rank + ' of ' + self.suit
class Deck():
    def __init__(self):
        self.deck = []
        for s in suits:
            for r in ranks:
                self.deck.append(Card(s,r))
    def __str__(self):
        deck_comp = ''  
        for card in self.deck:
            deck_comp += '\n '+card.__str__() 
        return 'The deck has:' + deck_comp
    def shuffle(self):
        random.shuffle(self.deck)
    def deal(self):
        single_card = self.deck.pop(0)
        return single_card
    class Hand():
    def __init__(self):
        self.cards = []
        self.value = 0
        self.aces = 0
    def add_card(self,card):
        self.cards.append(card) #taking in a card object
        self.value += values[card.rank] #adding to the total value of the hand
        if card.rank == 'Ace':
            self.aces += 1
    def adjust_for_ace(self):
        while self.value > 21 and self.aces > 0:
            self.value -= 10
            self.aces -= 1
class Chips():
    def __init__(self,total = 100):
        self.total = total
        self.bet = 0
    def win_bet(self):
        self.total += self.bet
    def lose_bet(self):
        self.total -= self.bet
def take_bet(chips):
    while True:
        try:
            chips.bet = int(input('How many chips would you like to bet? '))
        except:
            print('Sorry! Please provide an integer')
        else:
            if chips.bet > chips.total:
                    print('Sorry, you don not have enough chips.\nYou have {}.'.format(chips.total))
            else:
                break
def hit(deck,hand):
    single_card = deck.deal()
    hand.add_card(single_card)
    hand.adjust_for_ace()
def hit_or_stand(deck,hand):
    global playing
    while True:
        x = input('Hit or stand?\nEnter h or s.')
        if x[0].lower == 'h':
            hit(deck,hand)
        elif x[0] == 's':
            print('Player stands, dealers turn')
            playing = False
        else:
            print('Sorry, I did not understand.\nPlease enter h or s only.')
            continue
    
        break
def show_some(player,dealer):
    print('\nDealers hand: ')
    print('First card hidden.')
    print(dealer.cards[1])
    
    print('\nPlayers hand: ')
    for c in player.cards:
        print(c)
        
def show_all(player,dealer):
    
    print('Dealers hand: ')
    for c in dealer.cards:
        print(c)
    print(f'Value of dealers hand is {dealer.value}')
    
    
    print('Players hand: ')
    for c in player.cards:
        print(c)
    print(f'Value of players hand is {player.value}')
    
def player_busts(player,dealer,chips):
    print('Bust player!')
    chips.lose_bet()

def player_wins(player,dealer,chips):
    print('Player wins!')
    chips.win_bet()
    
def dealer_busts(player,dealer,chips):
    print('Player wins! Dealer busted.')
    chips.win_bet()
    
def dealer_wins(player,dealer,chips):
    print('Dealer wins!')
    chips.lose_bet()
    
    
def push(player,dealer):
    print('Dealer and player tie!\nPUSH!')
while True:
    print('WELCOME TO BLACKJACK!')
    deck = Deck()
    deck.shuffle()
   
    player_hand = Hand()
    player_hand.add_card(deck.deal())
    player_hand.add_card(deck.deal())
    
    dealer_hand = Hand()
    dealer_hand.add_card(deck.deal())
    dealer_hand.add_card(deck.deal())
    
    player_chips = Chips()
    take_bet(player_chips)
    
    show_some(player_hand,dealer_hand)
    
    while playing:
        hit_or_stand(deck,player_hand)
        
        show_some(deck,player_hand)
        
        if player_hand.value > 21:
            player_busts(player_hand,dealer_hand,player_chips)
            break
            
    if player_hand.value <= 21:
        while dealer_hand.value < 17:
            hit(deck,dealer_hand)
        
        show_all(player_hand,dealer_hand)
        
        if dealer_hand.value > 21:
            dealer_busts(player_hand,dealer_hand,player_chips)
        elif dealer_hand.value > player_hand.value:
            dealer_wins(player_hand,dealer_hand,player_chips)
        elif dealer_hand.value < player_hand.value:
            player_wins(player_hand,dealer_hand,player_chips)
        else:
            push(player_hand,dealer_hand)
            
    print('\nPlayers total remaining chips are at: {}'.format(player_chips.total))
    
    ng = input('Would you like to play another hand?\nEnter Y or N')
    if ng == 'Y':
        playing = True
        continue
    else:
        print('Thank you for playing!')
        break
        
 
