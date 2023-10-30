import telebot
from telebot import types

bot = telebot.TeleBot('6954975665:AAGAG1bwSHeZKYCeJSPNT5CZ31YjxW6scz4')

user_data = {}

# Вопросы и ответы
questions = {
    "1.1.Ты спал более 6 и менее 8 часов ?": None,
    "1.2.Ты питался здоровой едой от 4х раз в день, держа общую сумму калорий не меньше 3.400 ккал за сутки ?": None,
    "1.3.Ты пил от 2.8 литров жидкости в течение 24 часов ?": None,
    "1.4.Ты заниматься уходом за лицом (Выщипать бороду/Брови, чистка зубов (Дважды), крем и очиститель лица кожи(пенка))": None,
    "1.5.Ты занимался медитацией от 20ти минут в день (Отдых под энигму в наушниках) ?": None,
    "1.6.Ты не пил в течение этого времени ничего крепкого спиртного ?": None,
    "1.7.Ты не курил никакие виды сигарет и подобного ?": None,
    "1.8.Ты культурно развивался ежедневно? (Ходил куда-то (гулял по парку, ходил в музей, ходил в библиотеку, ходил по магазинам, ходил на свидания, играл в игры,смотрел интересный развлекательный контент, отдыхал))": None,
    "1.9.Ты Занимался активной физической нагрузкой? (Сауна/Секс/Тренировка/Растяжка)": None,

    "2.1.Ты Развивал насмотренность в дизайне? (Приложений/Баннеров/Сайтов)": None,
    "2.2.Ты делал по одной части кейсов по таргету? (Креатив/Наполнение и тп) ": None,
    "2.3.Ты придерживался ли ты общего плана о структурировании рабочего процесса - ежедневно работал с ним и улучшал результаты ?": None,

    "3.1.Ты учил английские слова, слушал английскую речь, подкасты, фильмы? У тебя был только английский в перерывах, когда сам ? ": None,
    "3.2.Ты узнавал больше про базы данных ?": None,
    "3.3.Ты много изучал машин лернинг для дата сайнтист ?": None,
    "3.4.Ты Читал книгу про Базы данных ?": None,
}

current_user = None
current_question = None

@bot.message_handler(commands=['start'])
def start(message):
    global current_user, current_question
    current_user = message.from_user.id
    user_data[current_user] = {}
    user_data[current_user]['answers'] = {}
    bot.send_message(current_user, "Привет! Это опрос для челленджа 28.10_30.11. Отвечай 'Да' или 'Нет' на вопросы.")
    current_question = 0
    ask_question()

@bot.message_handler(func=lambda message: message.text == 'Заново')
def restart(message):
    global current_user, current_question
    user_data[current_user] = {}
    user_data[current_user]['answers'] = {}
    current_question = 0
    ask_question()

def ask_question():
    global current_user, current_question
    if current_question is None:
        current_question = 0
    if current_question < len(questions):
        question_text = list(questions.keys())[current_question]
        markup = types.ReplyKeyboardMarkup(one_time_keyboard=True)
        markup.add(types.KeyboardButton('Да'))
        markup.add(types.KeyboardButton('Нет'))
        markup.add(types.KeyboardButton('Заново'))
        bot.send_message(current_user, question_text, reply_markup=markup)
    else:
        current_question = None
        send_answers()

@bot.message_handler(func=lambda message: True)
def handle_user_data(message):
    global current_user, current_question
    if current_user is not None:
        if current_question is not None:
            answer = message.text.strip().lower()
            if answer == "да" or answer == "нет":
                question_text = list(questions.keys())[current_question]
                user_data[current_user]['answers'][question_text] = "✅Да" if answer == "да" else "❌Нет"
                current_question += 1
                ask_question()
            else:
                bot.send_message(current_user, "Пожалуйста, ответь 'да' или 'нет' на вопрос.")
        else:
            bot.send_message(current_user, "Спасибо за ответы!")
            bot.send_message(current_user, "Если хочешь начать опрос заново, введи 'Заново'.")
            current_user = None
            current_question = None

def send_answers():
    global current_user
    if current_user is not None:
        answers = user_data[current_user]['answers']
        response = "Вот твои ответы:\n"
        for question, answer in answers.items():
            response += f"{question}: {answer}\n"
        bot.send_message(current_user, response)
        bot.send_message(current_user, "Если хочешь начать опрос снова, введи /start .")
        current_user = None

bot.polling(none_stop=True)
