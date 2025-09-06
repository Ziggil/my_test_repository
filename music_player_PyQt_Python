#прилоежение плеер на PyQt для Pycharm Python
import sys
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QHBoxLayout, QPushButton, QLabel, QSlider, QFileDialog, \
    QListWidget, QListWidgetItem
from PyQt5.QtGui import QIcon
from PyQt5.QtCore import Qt, QUrl
from PyQt5.QtMultimedia import QMediaPlayer, QMediaContent


class MusicPlayer(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle('Простой музыкальный плеер')
        self.setGeometry(100, 100, 400, 200)

        self.initUI()
        self.load_playlist()  # Загрузка плейлиста при запуске

    def initUI(self):
        self.player = QMediaPlayer()
        self.playlist = []

        # Создание кнопок управления
        self.play_button = QPushButton('Воспроизвести')
        self.play_button.clicked.connect(self.play_music)

        self.pause_button = QPushButton('Пауза')
        self.pause_button.clicked.connect(self.pause_music)

        self.stop_button = QPushButton('Стоп')
        self.stop_button.clicked.connect(self.stop_music)

        # Создание слайдера громкости
        self.volume_slider = QSlider(Qt.Horizontal)
        self.volume_slider.setValue(50)
        self.volume_slider.setMaximum(100)
        self.volume_slider.setToolTip('Громкость')
        self.volume_slider.valueChanged.connect(self.set_volume)

        # Создание слайдера перемотки трека
        self.position_slider = QSlider(Qt.Horizontal)
        self.position_slider.setToolTip('Позиция')
        self.position_slider.sliderMoved.connect(self.set_position)

        # Создание кнопок перемотки трека
        self.backward_button = QPushButton('<< 10 сек')
        self.backward_button.clicked.connect(self.backward_music)

        self.forward_button = QPushButton('10 сек >>')
        self.forward_button.clicked.connect(self.forward_music)

        # Кнопка выбора файла
        self.select_file_button = QPushButton('Добавить файл в плейлист')
        self.select_file_button.clicked.connect(self.select_file)

        # Кнопка очистки плейлиста
        self.clear_playlist_button = QPushButton('Очистить плейлист')
        self.clear_playlist_button.clicked.connect(self.clear_playlist)

        # Список воспроизведения
        self.playlist_widget = QListWidget()
        self.playlist_widget.itemDoubleClicked.connect(self.play_selected_track)

        # Метка для отображения информации о треке
        self.track_label = QLabel('Название трека')

        # Создание компоновщика
        vbox = QVBoxLayout()
        hbox1 = QHBoxLayout()
        hbox2 = QHBoxLayout()
        hbox3 = QHBoxLayout()

        hbox1.addWidget(self.play_button)
        hbox1.addWidget(self.pause_button)
        hbox1.addWidget(self.stop_button)
        hbox1.addWidget(self.volume_slider)

        hbox2.addWidget(self.backward_button)
        hbox2.addWidget(self.position_slider)
        hbox2.addWidget(self.forward_button)

        hbox3.addWidget(self.track_label)
        hbox3.addWidget(self.select_file_button)
        hbox3.addWidget(self.clear_playlist_button)

        vbox.addWidget(self.playlist_widget)
        vbox.addLayout(hbox3)
        vbox.addLayout(hbox1)
        vbox.addLayout(hbox2)

        self.setLayout(vbox)

        # Применение стилей к элементам
        self.apply_styles()

    def apply_styles(self):
        style = """
            QPushButton {
                background-color: #4CAF50;
                border: none;
                color: white;
                padding: 10px 24px;
                text-align: center;
                text-decoration: none;
                display: inline-block;
                font-size: 16px;
                margin: 4px 2px;
                transition-duration: 0.4s;
                cursor: pointer;
                border-radius: 12px;
            }

            QPushButton:hover {
                background-color: #45a049;
                color: white;
            }

            QSlider::handle:horizontal {
                background-color: #4CAF50;
                border: 2px solid #4CAF50;
                width: 16px;
                margin-top: -5px;
                margin-bottom: -5px;
                border-radius: 8px;
            }

            QSlider::groove:horizontal {
                border: 1px solid #A0A0A0;
                height: 10px;
                margin: 0px;
                border-radius: 5px;
            }

            QSlider::sub-page:horizontal {
                background: #4CAF50;
                border-radius: 5px;
            }
        """

        self.setStyleSheet(style)

    def select_file(self):
        file_path, _ = QFileDialog.getOpenFileName(self, "Выбрать музыкальный файл", "",
                                                   "Музыкальные файлы (*.mp3 *.wav *.ogg)")
        if file_path:
            self.playlist.append(file_path)
            item = QListWidgetItem(file_path)
            self.playlist_widget.addItem(item)
            self.save_playlist()  # Сохранение плейлиста после добавления трека

    def play_music(self):
        if self.playlist:
            url = QUrl.fromLocalFile(self.playlist[0])
            content = QMediaContent(url)
            self.player.setMedia(content)
            self.player.play()

    def play_selected_track(self, item):
        index = self.playlist_widget.row(item)
        if index >= 0 and index < len(self.playlist):
            url = QUrl.fromLocalFile(self.playlist[index])
            content = QMediaContent(url)
            self.player.setMedia(content)
            self.player.play()

    def pause_music(self):
        self.player.pause()

    def stop_music(self):
        self.player.stop()

    def set_volume(self):
        volume = self.volume_slider.value()
        self.player.setVolume(volume)

    def set_position(self, position):
        self.player.setPosition(position)

    def backward_music(self):
        position = max(0, self.player.position() - 10000)
        self.player.setPosition(position)

    def forward_music(self):
        position = min(self.player.duration(), self.player.position() + 10000)
        self.player.setPosition(position)

    def clear_playlist(self):
        self.playlist_widget.clear()
        self.playlist.clear()
        self.save_playlist()  # Сохранение плейлиста после очистки

    def save_playlist(self):
        with open('playlist.txt', 'w') as f:
            for track in self.playlist:
                f.write(track + '\n')

    def load_playlist(self):
        try:
            with open('playlist.txt', 'r') as f:
                self.playlist = [line.strip() for line in f.readlines()]
                for track in self.playlist:
                    item = QListWidgetItem(track)
                    self.playlist_widget.addItem(item)
        except FileNotFoundError:
            # Если файл не найден, ничего не делаем
            pass


if __name__ == '__main__':
    app = QApplication(sys.argv)
    player = MusicPlayer()
    player.show()
    sys.exit(app.exec_())
