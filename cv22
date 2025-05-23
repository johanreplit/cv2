import cv2
import pygame
import numpy as np

#Inisialisasi pygame
pygame.init()

#Webcam
cap = cv2.VideoCapture(0)

#Simpan file foto
nama_file = "foto"

#Nomor foto
nomor_foto = 0
gambar = pygame.image.load(r"C:\Users\Johan\Downloads\bingkai[1].png")
#Menyimpan status pengamblan foto
foto_diambil = False
foto_timer = 0#Timer menampilkan teks foto
freeze_frame = None#Simpan frame
#font dan ukuran layar
font = pygame.font.Font(None,64)#Ukuran font perbersar
font_small = pygame.font.Font(None,32)
screen = pygame.display.set_mode((640, 550))
pygame.display.set_caption("Take Photos")
while True:
    #Baca frame dari webcam
    ret, frame_opencv = cap.read()

    if not ret: #Jika foto gagal diambil maka akan lanjut ke program selanjutnya
        continue
    #konversi frame ke format rgb sebelum freeze
    frame_opencv = cv2.cvtColor(frame_opencv, cv2.COLOR_BGR2RGB)

    if foto_diambil and pygame.time.get_ticks() - foto_timer < 2000:
        frame_opencv = freeze_frame#Frame RGB yang telah disimpan saat foto itu diambil
    else:
        foto_diambil = False#Reset stauts jika sudah lewat 2 detik
    
    frame_pygame = np.rot90(frame_opencv)
    frame_pygame = pygame.surfarray.make_surface(frame_pygame)
    frame_pygame = pygame.transform.scale(frame_pygame, (640, 480))

    screen.blit(frame_pygame, (0, 0))
    bingkai = pygame.transform.scale(gambar, (640,480))
    screen.blit(bingkai, (0, 0))

    text_intruksi = font_small.render("= tekan SPACE untuk mengambil foto", True, (255,255,255))
    screen.blit(text_intruksi, (100, 500))

    if foto_diambil and pygame.time.get_ticks() - foto_timer < 2000:
        text = font.render("Foto Tersimpan!", True, (255, 0, 0))
        text_rect = text.get_rect(center=(320, 50))
        screen.blit(text, text_rect)
    
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            cap.release()
            pygame.quit()
            quit()
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                #Simpan foto
                cv2.imwrite(f"C:\\Users\\Johan\\Downloads\\{nama_file}_{nomor_foto}.jpg",
                            cv2.cvtColor(frame_opencv, cv2.COLOR_RGB2BGR))
                foto_diambil = True
                foto_timer = pygame.time.get_ticks()
                freeze_frame = frame_opencv.copy()
                nomor_foto += 1
    pygame.display.flip()
