import tkinter as tk
from tkinter import ttk
from tkinter import font
import os

class RadioHobbyistApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Справочник Радиолюбителя")
        self.root.geometry("520x620")
        self.root.resizable(False, False)

        # --- Иконка окна ---
        try:
            if os.path.exists("icon.ico"):
                self.root.iconbitmap("icon.ico")
            else:
                print("Предупреждение: Файл 'icon.ico' не найден. Используется стандартная иконка.")
        except tk.TclError:
            print("Ошибка при установке иконки. Используется стандартная иконка.")

        # --- Стилизация и шрифты ---
        self.style = ttk.Style()
        try:
            self.base_font = font.Font(family="Segoe UI", size=10)
            self.bold_font = font.Font(family="Segoe UI", size=10, weight="bold")
            self.title_font = font.Font(family="Segoe UI", size=16, weight="bold")
            self.results_font = font.Font(family="Consolas", size=10)
            self.history_font = font.Font(family="Segoe UI", size=9, slant="italic")
            self.history_label_font = font.Font(family="Segoe UI", size=11, weight="bold")
            self.note_font = font.Font(family="Segoe UI", size=9, slant="italic", foreground="grey")
        except tk.TclError:
            # Запасные шрифты, если Segoe UI недоступен
            self.base_font = font.Font(family="Helvetica", size=10)
            self.bold_font = font.Font(family="Helvetica", size=10, weight="bold")
            self.title_font = font.Font(family="Helvetica", size=16, weight="bold")
            self.results_font = font.Font(family="Courier New", size=10)
            self.history_font = font.Font(family="Helvetica", size=9, slant="italic")
            self.history_label_font = font.Font(family="Helvetica", size=11, weight="bold")
            self.note_font = font.Font(family="Helvetica", size=9, slant="italic", weight="normal")

        self.style.configure("TLabel", font=self.base_font, padding=2, background="#f0f0f0")
        self.style.configure("TButton", font=self.bold_font, padding=(10, 3))
        self.style.configure("TEntry", font=self.base_font, padding=5)
        self.style.configure("TFrame", background="#f0f0f0")
        self.style.configure("Clear.TButton", foreground="#d32f2f", font=self.bold_font)

        # --- Главный фрейм ---
        main_frame = ttk.Frame(root, padding="20", style="TFrame")
        main_frame.pack(expand=True, fill=tk.BOTH)

        # --- Заголовок ---
        title_frame = ttk.Frame(main_frame, style="TFrame")
        title_frame.pack(fill=tk.X, pady=(0, 20))

        title_label = ttk.Label(title_frame, text="Справочник Радиолюбителя", font=self.title_font, anchor="center")
        title_label.pack(side=tk.LEFT, expand=True)

        # --- Фрейм для иконки словаря ---
        # Помещаем иконку словаря в отдельный фрейм, чтобы можно было ее позиционировать
        dictionary_icon_frame = ttk.Frame(title_frame, style="TFrame")
        dictionary_icon_frame.pack(side=tk.RIGHT, padx=(10, 0))

        # Иконка "Книжечки"
        self.dictionary_label = tk.Label(dictionary_icon_frame, text="📖", font=("Arial", 20), cursor="hand2", bg="#f0f0f0")
        self.dictionary_label.pack()
        self.dictionary_label.bind("<Enter>", self.show_dictionary_popup)

        # --- Фрейм для ввода ---
        input_frame = ttk.Frame(main_frame, padding="10", style="TFrame")
        input_frame.pack(fill=tk.X, pady=5)

        self.query_label = ttk.Label(input_frame, text="Поиск:", style="TLabel")
        self.query_label.pack(side=tk.LEFT, padx=(0, 10))

        self.query_entry = ttk.Entry(input_frame, width=25, style="TEntry")
        self.query_entry.pack(side=tk.LEFT, expand=True, fill=tk.X, ipadx=4, ipady=4)
        self.query_entry.bind("<Return>", lambda event=None: self.search_item())

        self.clear_button = ttk.Button(input_frame, text="Очистить", command=self.clear_all, style="Clear.TButton")
        self.clear_button.pack(side=tk.LEFT, padx=(10, 5))

        self.search_button = ttk.Button(input_frame, text="Найти", command=self.search_item)
        self.search_button.pack(side=tk.LEFT, padx=(5, 0))

        # --- Фрейм для результатов ---
        results_frame = ttk.Frame(main_frame, padding="10", style="TFrame")
        results_frame.pack(expand=True, fill=tk.BOTH, pady=10)

        self.results_text = tk.Text(results_frame, wrap=tk.WORD, height=10,
                                     font=self.results_font, borderwidth=2, relief="groove",
                                     padx=8, pady=8, background="white")
        self.results_text.pack(expand=True, fill=tk.BOTH)

        # --- Фрейм для истории запросов ---
        history_frame = ttk.Frame(main_frame, padding="10", style="TFrame")
        history_frame.pack(fill=tk.X, pady=5)

        history_title_label = ttk.Label(history_frame, text="История поисков:", font=self.history_label_font)
        history_title_label.pack(anchor=tk.W, pady=(0, 5))

        self.history_text = tk.Text(history_frame, wrap=tk.WORD, height=5,
                                     font=self.history_font, borderwidth=1, relief="flat",
                                     padx=8, pady=8, background="#e8e8e8")
        self.history_text.pack(fill=tk.X)
        self.history_text.configure(state='disabled')

        # --- База данных ---
        self.data = {
            "Резистор": "Пассивный электронный компонент, основное назначение которого — противодействовать электрическому току. Измеряется в Омах (Ом).",
            "Резистор постоянный": "Резистор с фиксированным значением сопротивления.",
            "Резистор подстроечный": "Резистор, сопротивление которого можно плавно регулировать для точной настройки схемы. Имеет 3 вывода.",
            "Резистор переменный": "Резистор, сопротивление которого можно изменять во время работы схемы (например, регулятор громкости), обычно вращением ручки. Также известен как потенциометр.",
            "Транзистор": "Полупроводниковый прибор, который можно использовать для усиления, генерации и переключения электронных сигналов и электрической мощности. Имеет как минимум 3 вывода (база, эмиттер, коллектор).",
            "Конденсатор": "Накапливает электрический заряд и энергию электрического поля. Характеризуется ёмкостью (измеряется в Фарадах, Ф).",
            "Диод": "Полупроводниковый прибор с одним p-n переходом, пропускающий ток преимущественно в одном направлении.",
            "Индуктивность": "Электронный компонент (катушка), который запасает энергию в магнитном поле при протекании через него электрического тока. Измеряется в Генри (Гн).",
            "Микросхема (ИС)": "Интегральная схема, содержит множество транзисторов, резисторов и конденсаторов на одном кристалле.",
            "Операционный усилитель (ОУ)": "Схематично обозначаемый как ОУ, это усилитель постоянного тока с высоким коэффициентом усиления, дифференциальным входом и, как правило, одним выходом.",
            "Стабилизатор напряжения": "Электронная схема, которая поддерживает постоянный уровень выходного напряжения независимо от изменений входного напряжения или нагрузки.",
            "Кварцевый резонатор": "Электронный компонент, который использует механические резонансные свойства кварцевого кристалла для генерации очень стабильного тактового сигнала.",
            "Плата (PCB)": "Печатная плата (Printed Circuit Board), основа для монтажа электронных компонентов.",
            "Макетная плата (Breadboard)": "Многоразовая плата для прототипирования электронных схем без пайки.",
            "Пайка": "Процесс соединения металлических частей с помощью припоя, который плавится под действием нагрева.",
            "Припой": "Легкоплавкий сплав, используемый для соединения электронных компонентов и проводников.",
            "Флюс": "Вещество, которое удаляет оксиды с поверхностей, подготавливая их к пайке.",
            "Разъем (Коннектор)": "Устройство, позволяющее соединять два провода или кабеля, или проводник с устройством.",
            "Антенна": "Устройство, предназначенное для излучения или приема радиоволн.",
            "Переключатель (Свитч)": "Устройство для замыкания или размыкания электрической цепи.",
            "SPDT": "Однополюсный двухпозиционный переключатель (Single Pole Double Throw).",
            "DPDT": "Двухполюсный двухпозиционный переключатель (Double Pole Double Throw).",
            "Реле": "Электромагнитное устройство, позволяющее коммутировать электрические цепи при помощи управляющего сигнала.",
            "Батарея (Аккумулятор)": "Устройство, преобразующее химическую энергию в электрическую.",
            "Источник питания": "Устройство, обеспечивающее электрической энергией другие устройства.",
            "Осциллограф": "Прибор для наблюдения и измерения электрических сигналов в виде графика.",
            "Мультиметр": "Измерительный прибор, объединяющий несколько функций: вольтметр, амперметр, омметр и др.",
            "Генератор сигналов": "Прибор, предназначенный для создания электрических сигналов заданной формы и параметров.",
            "Термоусадочная трубка": "Трубка из термопластичного материала, которая сжимается под действием нагрева, обеспечивая изоляцию и фиксацию.",
            "Провод": "Гибкий проводник из металла (часто меди) для передачи электрического тока.",
            "Кабель": "Совокупность проводов или жил, заключенная в общую оболочку, иногда с экранированием.",
            "Светодиод (LED)": "Полупроводниковый прибор, излучающий свет при протекании через него электрического тока.",
            "Потенциометр": "Регулируемый резистор с тремя выводами, используемый для создания переменного напряжения. Является разновидностью переменного резистора.",
            "Трансформатор": "Устройство для преобразования переменного тока одного напряжения в переменный ток другого напряжения.",
            "SMD": "Компонент для поверхностного монтажа (Surface Mount Device).",
            "THT": "Компонент для монтажа в отверстия (Through-Hole Technology).",
            "Даташит (Datasheet)": "Техническая документация на электронный компонент, содержащая его характеристики и параметры.",
            "Симистор": "Двунаправленный тиристор, используемый для коммутации переменного тока.",
            "Тиристор": "Полупроводниковый прибор, который может длительное время проводить ток в одном направлении при наличии управляющего сигнала.",
            "ШИМ (PWM)": "Широтно-импульсная модуляция, метод управления мощностью или уровнем сигнала путем изменения скважности импульсов.",
            "RC-цепь": "Схема, состоящая из резистора (R) и конденсатора (C), используемая для фильтрации сигналов или формирования задержек.",
            "RL-цепь": "Схема, состоящая из резистора (R) и индуктивности (L), используемая для фильтрации или ограничения тока.",
            "LC-цепь": "Схема, состоящая из индуктивности (L) и конденсатора (C), используемая для создания резонансных контуров и фильтров.",
            "Фоторезистор": "Резистор, сопротивление которого изменяется в зависимости от уровня освещенности.",
            "Варистор": "Компонент, сопротивление которого резко уменьшается при превышении определенного напряжения, используется для защиты от перенапряжения.",
            "Термопара": "Датчик температуры, основанный на эффекте Зеебека, генерирующий небольшое напряжение, пропорциональное разнице температур между двумя разнородными проводниками.",
            "Эмиттер": "Один из трех основных выводов биполярного транзистора, участвующий в инжекции носителей заряда.",
            "База": "Управляющий вывод биполярного транзистора, контролирующий ток между коллектором и эмиттером.",
            "Коллектор": "Один из трех основных выводов биполярного транзистора, через который выходит основной поток носителей заряда.",
            "Разрядник": "Устройство, предназначенное для защиты от перенапряжений путем создания контролируемого пути для разряда (например, газовый разрядник).",
            "Штекер": "Разъем, который вставляется в гнездо (розетку) для соединения электрических цепей.",
            "Клемма": "Металлическая часть для соединения проводов, обычно с винтовым креплением.",
            "Силовой трансформатор": "Трансформатор, предназначенный для изменения напряжений в силовых цепях, например, в блоках питания.",
            "Развязывающий трансформатор": "Трансформатор, обеспечивающий гальваническую развязку между первичной и вторичной обмотками для повышения безопасности."
        }
        self.query_history = [] # Список для хранения истории запросов

        # --- Переменные для всплывающего окна словаря ---
        self.dictionary_popup = None
        self.hide_timer = None # Таймер для скрытия всплывающего окна

    def update_history(self, query):
        """Добавляет запрос в историю и ограничивает ее размер."""
        self.query_history.insert(0, query)
        if len(self.query_history) > 10:
            self.query_history.pop()

        self.history_text.configure(state='normal')
        self.history_text.delete(1.0, tk.END)
        for i, item in enumerate(self.query_history):
            self.history_text.insert(tk.END, f"{i+1}. {item}\n")
        self.history_text.configure(state='disabled')

    def clear_all(self):
        """Очищает поле ввода и область результатов."""
        self.query_entry.delete(0, tk.END)
        self.results_text.delete(1.0, tk.END)
        self.results_text.insert(tk.END, "Поле ввода и результаты очищены.")

    def search_item(self):
        """Выполняет поиск в базе данных, учитывая регистр и частичные совпадения."""
        query = self.query_entry.get().strip()
        if not query:
            self.results_text.delete(1.0, tk.END)
            self.results_text.insert(tk.END, "Поле поиска не может быть пустым.")
            return

        normalized_query = query.lower()
        found_item = None
        exact_match_key = None
        partial_match_key = None

        # 1. Поиск точного совпадения (без учета регистра)
        for key in self.data.keys():
            if key.lower() == normalized_query:
                found_item = self.data[key]
                exact_match_key = key
                break

        # 2. Если точного совпадения нет, ищем частичное совпадение (без учета регистра)
        if found_item is None:
            for key in self.data.keys():
                if normalized_query in key.lower():
                    found_item = self.data[key]
                    partial_match_key = key
                    break

        # --- Отображение результата ---
        self.results_text.delete(1.0, tk.END)
        if found_item:
            display_result = ""
            if exact_match_key:
                display_result = f"-> {exact_match_key.capitalize()}:\n\n{found_item}"
            elif partial_match_key:
                display_result = f"-> Результат для '{query}' (найдено частичное совпадение с '{partial_match_key.capitalize()}'):\n\n{found_item}"
            self.results_text.insert(tk.END, display_result)
            self.update_history(query)
        else:
            self.results_text.insert(tk.END, "Информация не найдена.")
            self.update_history(query)

    # --- Функции для всплывающего окна словаря ---
    def show_dictionary_popup(self, event):
        """Показывает всплывающее окно со всеми словами из базы данных."""
        # Отменяем запланированное скрытие, если курсор вернулся
        if self.hide_timer:
            self.root.after_cancel(self.hide_timer)
            self.hide_timer = None

        # Создаем окно, если оно не существует или было закрыто
        if self.dictionary_popup is None or not self.dictionary_popup.winfo_exists():
            self.dictionary_popup = tk.Toplevel(self.root)
            self.dictionary_popup.wm_overrideredirect(True) # Убираем рамку и заголовок окна

            # Позиционируем окно у курсора с небольшим смещением
            x_pos = event.x_root + 10
            y_pos = event.y_root + 10
            self.dictionary_popup.geometry(f"+{x_pos}+{y_pos}")

            popup_frame = ttk.Frame(self.dictionary_popup, relief="solid", borderwidth=1, style="TFrame")
            popup_frame.pack(fill=tk.BOTH, expand=True)

            # Получаем и сортируем все ключи (слова)
            all_words = sorted(self.data.keys())
            max_word_len = 0
            if all_words:
                max_word_len = max(len(word) for word in all_words)

            # Определяем разумную ширину для текстового поля
            popup_text_width = min(max(max_word_len, 20), 45) # Минимум 20, максимум 45 символов

            # Создаем виджет Text для отображения списка слов
            list_text_widget = tk.Text(popup_frame, wrap=tk.WORD, height=min(len(all_words), 15), # Ограничиваем высоту
                                       font=self.base_font, padx=5, pady=5, bg="white", bd=0, # bd=0 убирает внутренний border Text
                                       width=popup_text_width)
            list_text_widget.insert(tk.END, "\n".join(all_words))
            list_text_widget.configure(state='disabled') # Делаем текст только для чтения
            list_text_widget.pack()

            # Привязываем события к самому всплывающему окну, чтобы оно корректно обрабатывало наведение курсора
            self.dictionary_popup.bind("<Enter>", self.show_dictionary_popup) # Если курсор входит в popup, поднимаем его
            self.dictionary_popup.bind("<Leave>", self.hide_dictionary_popup)      # Если курсор покидает popup, планируем его скрытие

        else:
            # Если окно уже существует, просто поднимаем его наверх
            self.dictionary_popup.lift()

    def hide_dictionary_popup(self, event=None):
        """Скрывает всплывающее окно со словами (через небольшой таймер)."""
        # Запускаем таймер. Если курсор вернется в окно или на иконку, таймер отменится.
        self.hide_timer = self.root.after(200, self._do_hide_popup) # Задержка 200 миллисекунд

    def _do_hide_popup(self):
        """Фактическое скрытие всплывающего окна."""
        if self.dictionary_popup and self.dictionary_popup.winfo_exists():
            self.dictionary_popup.withdraw() # Скрываем окно

# --- Инициализация и запуск приложения ---
if __name__ == "__main__":
    root = tk.Tk()
    app = RadioHobbyistApp(root)
    root.mainloop()
