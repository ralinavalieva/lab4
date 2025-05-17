1.
import random  # импорт. модуль для генерации случ чисел
import time    # импорт. модуль для работы со вр
import json    # импорт. модуль для работы с JSON
import os      # импорт. модуль для работы с файловой системой

class GuessingGame:
    def __init__(self):
        self.number_to_guess = random.randint(1, 100)  # загад случ число от 1 до 100
        self.attempts = 0                               # счетчик попыток
        self.start_time = None                          # вр начала игры

    def start(self):
        """запуск игру."""
        self.start_time = time.time()                   # запоминаем время начала игры
        print("я загадал число от 1 до 100. попробуйте угадать его!")

        while True:  # бесконечный цикл для угадывания числа
            try:
                guess = int(input("введите ваше предполож: "))  #запр у пользователя ввод числа
                self.attempts += 1  # увел счетчик попыток

                if guess < self.number_to_guess:
                    print("слишком мал число. попробуйте снова.")
                elif guess > self.number_to_guess:
                    print("слишком бол число. попробуйте снова.")
                else:
                    end_time = time.time()  # запоминаем время окончания игры
                    duration = end_time - self.start_time  # вычисляем продолжительность игры
                    print(f"поздравляю! Вы угадали число {self.number_to_guess} за {self.attempts} попыток.")
                    print(f"время игры: {duration:.2f} секунд.")

                    self.save_statistics(duration, "win")  # сохр статистику о выигрыше
                    break  # выходим из цикла после успешного угадывания
            except ValueError:
                print("пожалуйста, введите целое число.")  # обработка ошибки ввода

    def save_statistics(self, duration, result):
        """сохр статистику в файл."""
        stats = {
            "attempts": self.attempts,   # кол-во попыток
            "duration": duration,         # вр игры в сек
            "result": result              # рез-т игры (выигрыш или проигрыш)



        # проверяем, сущ ли файл статистики
        if os.path.exists("statistics.json"):
            with open("statistics.json", "r") as file:
                data = json.load(file)  # загр существующие данные из файла
                data.append(stats)       # доб нов запись о текущей игре
        else:
            data = [stats]              # если файла нет, созд нов список с 1 записью

        with open("statistics.json", "w") as file:
            json.dump(data, file, indent=4)  # сохр данные обратно в файл в формате JSON


if __name__ == "__main__":
    game = GuessingGame()  # созд экземпляр класса GuessingGame
    game.start()           # запускаем игру

    
2.
import random  # импорт модуль для генерации случ чисел


class BlackjackGame:
    def __init__(self):
        self.player_hand = []  # карты игрока
        self.dealer_hand = []  # карты дилера
        self.game_over = False  # флаг оконч игры

    def start(self):
        """запускает игру."""
        self.deal_initial_cards()  # раздаем началь карты
        print(f"Ваши карты: {self.player_hand}, сумма: {self.calculate_hand_value(self.player_hand)}")
        print(f"Карты дилера: [{self.dealer_hand[0]}, ?]")  # показ 1 карту дилера

        while not self.game_over:
            self.player_turn()  # ход игрока
            if not self.game_over:
                self.dealer_turn()  # ход дилера
                self.determine_winner()  # опр победителя

    def deal_initial_cards(self):
        """раздает началь карты игроку и дилеру."""
        for _ in range(2):
            self.player_hand.append(self.draw_card())  # игрок получ 2 карты
            self.dealer_hand.append(self.draw_card())  # дилер получ 2 карты

    def draw_card(self):
        """возвр случ знач карты от 2 до 11."""
        return random.randint(2, 11)

    def calculate_hand_value(self, hand):
        """вычисл сум карт в руке."""
        return sum(hand)

    def player_turn(self):
        """ход игрока."""
        while True:
            action = input("хотите взять карту (в) или остановиться (о)? ").strip().lower()
            if action == 'в':
                card = self.draw_card()
                self.player_hand.append(card)
                print(
                    f"вы взяли карту: {card}. ваши карты: {self.player_hand}, сумма: {self.calculate_hand_value(self.player_hand)}")

                if self.calculate_hand_value(self.player_hand) > 21:
                    print("перебор! вы проиграли.")
                    self.game_over = True
                    return
            elif action == 'о':
                break
            else:
                print("неверный ввод. пожалуйста, введите 'в' или 'о'.")

    def dealer_turn(self):
        """ход дилера."""
        while self.calculate_hand_value(self.dealer_hand) < 17:
            card = self.draw_card()
            self.dealer_hand.append(card)

        print(f"карты дилера: {self.dealer_hand}, сумма: {self.calculate_hand_value(self.dealer_hand)}")

    def determine_winner(self):
        """опр победителя."""
        player_value = self.calculate_hand_value(self.player_hand)
        dealer_value = self.calculate_hand_value(self.dealer_hand)

        if dealer_value > 21:
            print("дилер перебрал! вы выиграли!")
        elif player_value > dealer_value:
            print("поздравляем! вы выиграли!")
        elif player_value < dealer_value:
            print("дилер выиграл. вы проиграли.")
        else:
            print("ничья!")


if __name__ == "__main__":
    game = BlackjackGame()  # созд экземпляр класса
    game.start()  # запускаем игру
