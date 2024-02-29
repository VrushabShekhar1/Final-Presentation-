#Final Programming for Economists II Project
#Group Members: Sam Sohrab Junkermann, Lain Calvo Carrasco, Vukasin Tolic, Vrushab Shekhar
import random

class SlotMachine:
    def __init__(self):
        self.balance = 100
        self.symbols = ['Cherry', 'Apple', 'Banana', 'Orange', 'Lemon', 'Grape', 'Kiwi', 'Mango']
        self.cumulative_spins = 0

    def spin(self, doubling_option):
        self.balance -= 5
        self.cumulative_spins += 1
        result = [random.choice(self.symbols) for _ in range(5)]
        print(f"Spin #{self.cumulative_spins} | Balance: {self.balance} coins")
        print("-------------------------------------------")
        print("|", end="")
        for item in result:
            print(item, end=" | ")
        print("\n-------------------------------------------")

        # Check for different combinations and define payoffs
        winnings = 0
        if result.count(result[0]) == 5:  # 5 of a kind
            winnings += 100
            print("You got 5 of a kind! You win $100.")
        elif len(set(result)) == 2:  # Full House
            winnings += 50
            print("You got a Full House! You win $50.")
        elif 4 in [result.count(x) for x in set(result)]:  # 4 of a kind
            winnings += 25
            print("You got 4 of a kind! You win $25.")
        elif 3 in [result.count(x) for x in set(result)]:  # 3 of a kind
            winnings += 10
            print("You got 3 of a kind! You win $10.")
        elif len(set(result)) == 3:  # 2 pair
            winnings += 5
            print("You got 2 pairs! You win $5.")
        elif len(set(result)) == 4:  # 1 pair
            winnings += 2
            print("You got 1 pair! You win $2.")
        else:
            print("Sorry, you didn't win anything this time.")

        self.balance += winnings
        if winnings > 0 and doubling_option != 0:
            doubled_winnings = self.double_winnings(winnings, doubling_option)
            if doubled_winnings == 0:
                print("Your doubled winnings have been lost.")
            else:
                self.balance += doubled_winnings

    def play(self):
        choice = input("Would you like to play? (yes/no): ").lower()
        if choice != 'yes':
            print("Maybe next time!")
            return

        while self.balance > 0:
            print("\nBalance:", self.balance, "coins")
            play_option = input("Would you like to spin (s) or play multispins (m)? ").lower()
            if play_option == 's':
                doubling_option = input("Please select a predetermined doubling setup (0-5): ")
                if doubling_option not in ['0', '1', '2', '3', '4', '5']:
                    print("Invalid doubling option. Please choose between 0 and 5.")
                    continue
                doubling_option = int(doubling_option)
                self.spin(doubling_option)
            elif play_option == 'm':
                while True:
                    num_spins = int(input("How many spins would you like to play?: "))
                    doubling_option = input("Please select a predetermined doubling setup (0-5): ")
                    if doubling_option not in ['0', '1', '2', '3', '4', '5']:
                        print("Invalid doubling option. Please choose between 0 and 5.")
                        continue
                    doubling_option = int(doubling_option)
                    self.multispin(num_spins, doubling_option)
                    choice = input("Would you like to play again? (s/m): ").lower()
                    if choice not in ['s', 'm']:
                        print("Invalid input. Please enter 's' or 'm'.")
                        continue
                    elif choice == 's':
                        break
            else:
                print("Invalid input. Please enter 's' or 'm'.")

        print("You've run out of coins. Game over.")

    def double_winnings(self, current_winnings, doubling_setup):
        if current_winnings == 0:
            print("No winnings to double.")
            return 0

        probability = 0.5
        for _ in range(doubling_setup):
            if random.random() < probability:
                current_winnings *= 2
                print("Congratulations! Your winnings have been doubled.")
            else:
                print("Sorry, you lost all your doubled winnings.")
                current_winnings = 0
                break
            probability /= 2
        return current_winnings

    def multispin(self, num_spins, doubling_option):
        for _ in range(num_spins):
            if self.balance <= 0:
                print("You've run out of coins. Game over.")
                break
            print(f"\nDoubling setup: {doubling_option}")
            self.spin(doubling_option)

# Main program
if __name__ == "__main__":
    slot_machine = SlotMachine()
    slot_machine.play()
