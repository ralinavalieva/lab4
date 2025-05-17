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
