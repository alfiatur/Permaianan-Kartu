# Permaianan-Kartu
import random
import time # Untuk jeda waktu agar pesan bisa dibaca

def create_deck():
    """
    Membuat dek kartu berisi nama-nama bunga.
    Setiap bunga akan ada beberapa kali dalam dek.
    Kemudian mengocok dek.
    """
    # Daftar nama-nama bunga yang akan digunakan
    flower_names = [
        "Anggrek", "Anyelir", "Aster", "Bougenville", "Dahlia",
        "Edelweiss", "Freesia", "Gardenia", "Hydrangea", "Iris",
        "Jasmine", "Kamboja", "Lily", "Mawar", "Melati",
        "Orchid", "Peony", "Sakura", "Tulip", "Violet"
    ]
    
    deck = []
    # Tambahkan setiap bunga beberapa kali ke dek (misal 3 kali setiap bunga)
    for flower in flower_names:
        deck.extend([flower] * 3) 
    
    random.shuffle(deck)
    return deck

def display_message(message, message_type='info'):
    """
    Menampilkan pesan kepada pengguna dengan sedikit styling.
    """
    if message_type == 'error':
        print(f"\n--- ERROR ---\n{message}\n-------------")
    elif message_type == 'success':
        print(f"\n+++ BENAR! +++\n{message}\n--------------")
    else: # info
        print(f"\n--- INFO ---\n{message}\n------------")
    time.sleep(1.5) # Jeda agar pengguna bisa membaca pesan

def start_game():
    """
    Menginisialisasi permainan dan memulai putaran pertama.
    """
    global deck, current_card, score, game_active

    display_message("Memulai permainan Tebak Bunga!")
    deck = create_deck()
    score = 0
    game_active = True

    if not deck:
        display_message("Dek bunga kosong! Tidak bisa memulai permainan.", 'error')
        game_active = False
        return

    current_card = deck.pop(0) # Ambil bunga pertama
    print(f"\n--- Permainan Dimulai ---")
    print(f"Bunga saat ini: {current_card}")
    print(f"Skor Anda: {score}")

def play_turn():
    """
    Menjalankan satu putaran permainan: meminta tebakan, menarik bunga baru,
    dan memeriksa tebakan.
    """
    global current_card, score, game_active

    if not game_active:
        return

    if not deck:
        display_message("Dek bunga habis! Permainan berakhir.", 'error')
        game_active = False
        return

    print("\n-------------------------")
    print(f"Bunga saat ini: {current_card}")
    print(f"Skor: {score}")
    
    # Menjelaskan aturan "lebih tinggi" atau "lebih rendah" untuk string
    print("Urutan abjad (A-Z):")
    print("  'Lebih Tinggi' berarti bunga berikutnya muncul setelah bunga saat ini secara abjad.")
    print("  'Lebih Rendah' berarti bunga berikutnya muncul sebelum bunga saat ini secara abjad.")
    
    guess = input("Apakah bunga berikutnya akan (L)ebih Tinggi (secara abjad) atau (R)ebih Rendah (secara abjad)? (L/R): ").strip().lower()

    if guess not in ['l', 'r']:
        display_message("Tebakan tidak valid. Silakan masukkan 'L' atau 'R'.", 'info')
        return # Biarkan putaran ini berulang

    next_card = deck.pop(0) # Tarik bunga berikutnya
    print(f"Bunga berikutnya ditarik...")
    time.sleep(1) # Jeda untuk efek

    print(f"Bunga berikutnya adalah: {next_card}")

    correct_guess = False
    # Perbandingan string secara abjad
    if guess == 'l':
        if next_card >= current_card: # 'Lebih tinggi' berarti secara abjad sama atau setelahnya
            correct_guess = True
    elif guess == 'r':
        if next_card <= current_card: # 'Lebih rendah' berarti secara abjad sama atau sebelumnya
            correct_guess = True

    if correct_guess:
        score += 1
        current_card = next_card
        display_message(f"Benar! Skor Anda sekarang: {score}", 'success')
    else:
        display_message(f"Salah! Permainan berakhir. Bunga berikutnya adalah {next_card}. Skor akhir Anda: {score}", 'error')
        game_active = False

def main():
    """
    Fungsi utama untuk menjalankan loop permainan.
    """
    while True:
        start_game()
        while game_active:
            play_turn()
            if not game_active: # Periksa jika permainan berakhir setelah play_turn
                break

        play_again = input("\nIngin bermain lagi? (ya/tidak): ").strip().lower()
        if play_again != 'ya':
            print("Terima kasih sudah bermain! Sampai jumpa.")
            break

# Inisialisasi variabel global
deck = []
current_card = "" # Mengubah tipe data menjadi string
score = 0
game_active = False

# Jalankan permainan
if __name__ == "__main__":
    main()
