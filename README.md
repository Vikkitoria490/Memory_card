# Memory_card
from PyQt5.QtCore import Qt
from PyQt5.QtWidgets import QApplication, QWidget, QPushButton, QHBoxLayout, QVBoxLayout, QLabel, QGroupBox, QRadioButton, QButtonGroup
from random import shuffle, randint

class Question():
    def __init__(self, qst, right_answer, wrong1, wrong2, wrong3):
        self.qst = qst
        self.right_answer = right_answer
        self.wrong1 = wrong1
        self.wrong2 = wrong2
        self.wrong3 = wrong3

app = QApplication([])
main_win = QWidget()
main_win.setWindowTitle('Memory Card')
main_win.resize(500, 300)

ansBox = QGroupBox('Варианты ответа')

question = QLabel('Какой национальности не существует?')
btn_ok = QPushButton('Ответить')
main_lauout = QVBoxLayout()

btn_1 = QRadioButton('Энцы')
btn_2 = QRadioButton('Чулымцы')
btn_3 = QRadioButton('Смурфы')
btn_4 = QRadioButton('Алеуты')

layout = QVBoxLayout()
layout1 = QHBoxLayout()
layout2 = QHBoxLayout()

layout1.addWidget(btn_1)
layout1.addWidget(btn_2)
layout2.addWidget(btn_3)
layout2.addWidget(btn_4)

layout.addLayout(layout1)
layout.addLayout(layout2)

ansBox.setLayout(layout)

resBox = QGroupBox('Результаты теста')

result = QLabel('Правильно/Неправильно')
right_ans = QLabel('Правильный ответ')

layout_res = QVBoxLayout()
layout_res.addWidget(result)
layout_res.addWidget(right_ans, alignment=Qt.AlignCenter)
resBox.setLayout(layout_res)

layout_box = QHBoxLayout()
layout_box.addWidget(ansBox)
layout_box.addWidget(resBox)
ansBox.hide()


main_lauout.addWidget(question, stretch = 1)
main_lauout.addLayout(layout_box, stretch = 2)
main_lauout.addWidget(btn_ok, stretch = 2)

main_lauout.addWidget(question, alignment=Qt.AlignCenter)
main_lauout.addWidget(btn_ok, alignment=Qt.AlignCenter)

RadioGroup = QButtonGroup()
RadioGroup.addButton(btn_1)
RadioGroup.addButton(btn_2)
RadioGroup.addButton(btn_3)
RadioGroup.addButton(btn_4)

def show_results():
    ansBox.hide()
    resBox.show()
    btn_ok.setText('Следующий вопрос')

def show_question():
    resBox.hide()
    ansBox.show()
    btn_ok.setText('Ответить')
    RadioGroup.setExclusive(False)
    btn_1.setChecked(False)
    btn_2.setChecked(False)
    btn_3.setChecked(False)
    btn_4.setChecked(False)
    RadioGroup.setExclusive(True)

def start_test():
    if btn_ok.text() == 'Ответить':
        show_results()
    else:
        show_question()

answers = [btn_1, btn_2, btn_3, btn_4]

def ask(q):
    shuffle(answers)
    answers[0].setText(q.right_answer)
    answers[1].setText(q.wrong1)
    answers[2].setText(q.wrong2)
    answers[3].setText(q.wrong3)
    question.setText(q.qst)
    right_ans.setText(q.right_answer)
    show_question()

question_list = []
question_list.append(Question('Государственный язык Бразилии','Португальский', 'Испанский', 'Бразилийский', 'Итальянский'))
question_list.append(Question('Какого цвета нет на флаге России','Зелёного', 'Синего', 'Красного', 'Белого'))
question_list.append(Question('Столица Эфиопии','Адис-Абеба', 'Каир', 'Хартум', 'Абуджа'))
question_list.append(Question('Где находится Бразилия?', 'Южная Америка', 'Россия', 'Северная Америка', 'Азия'))
question_list.append(Question('Как называется место, где река впадает в другую реку, море или озеро?', 'Устье', 'Исток', 'Русло', 'Половодье'))
question_list.append(Question('В это озеро нашей страны впадает множество рек, а вытекает из него только одна — Ангара.', 'Байкал', 'Селигер','Ладожское', 'Виктория'))

def show_correct(res):
    result.setText(res)
    show_results()

def check_answers():
    if answers[0].isChecked():
        show_correct('Правильно!')
        main_win.score += 1
    elif answers[1].isChecked() or answers[2].isChecked() or answers[3].isChecked():
        show_correct('Неверно!')
    print('Всех вопросов:', main_win.total, 'Правильных ответов:', main_win.score)
    print('Статистика:', main_win.score/main_win.total*100)

def next_question():
    main_win.total += 1
    cur_question = randint(0, len(question_list) - 1)
    q = question_list[cur_question]
    ask(q)

def click_ok():
    if btn_ok.text() == 'Ответить':
        check_answers()
    else:
        next_question()

main_win.cur_question = -1 
btn_ok.clicked.connect(click_ok)
main_win.total = 0
main_win.score = 0
next_question()
main_win.setLayout(main_lauout)
main_win.show()
app.exec_()
