- 💞️ I’m looking to collaborate on hert 3d
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
from mpl_toolkits.mplot3d import Axes3D
import pyaudio
import struct

# Hàm để đọc âm thanh từ microphone
def audio_input(callback, num_samples=1024, sample_rate=44100):
    p = pyaudio.PyAudio()
    stream = p.open(format=pyaudio.paInt16, channels=1, rate=sample_rate, input=True, frames_per_buffer=num_samples)
    while True:
        data = stream.read(num_samples)
        data_int = struct.unpack(str(num_samples) + 'h', data)
        callback(data_int)

# Hàm để vẽ trái tim 3D
def draw_heart():
    # Tạo dữ liệu cho trái tim
    t = np.linspace(0, 2*np.pi, 100)
    x = 16 * np.sin(t)**3
    y = 13 * np.cos(t) - 5 * np.cos(2*t) - 2 * np.cos(3*t) - np.cos(4*t)
    z = np.sin(3*t)
    
    # Tạo hình 3D
    fig = plt.figure()
    ax = fig.add_subplot(111, projection='3d')
    
    # Vẽ trái tim ban đầu với màu đỏ
    heart_line = ax.plot(x, y, z, color='red')[0]

    # Đặt tên cho các trục
    ax.set_xlabel('X')
    ax.set_ylabel('Y')
    ax.set_zlabel('Z')

    # Hàm callback để cập nhật màu sắc của trái tim dựa trên âm thanh
    def update_color(data):
        rms = np.sqrt(np.mean(np.square(data)))
        color = plt.cm.jet(rms/1000)  # Thay đổi màu sắc dựa trên âm lượng
        heart_line.set_color(color)
    
    # Gọi hàm audio_input để nhận dữ liệu từ microphone và cập nhật màu sắc của trái tim
    audio_input(update_color)
    
    plt.show()

# Gọi hàm để vẽ trái tim 3D
draw_heart()
