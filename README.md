# PBL
import pickle
import os
from datetime import datetime

# File untuk menyimpan data
USER_DB = "users.pkl"
LOGIN_HISTORY = "login_history.pkl"

# Memuat data dari file
if os.path.exists(USER_DB):
    with open(USER_DB, "rb") as file:
        users = pickle.load(file)
else:
    users = {}

if os.path.exists(LOGIN_HISTORY):
    with open(LOGIN_HISTORY, "rb") as file:
        login_history = pickle.load(file)
else:
    login_history = []

def save_data():
    """Menyimpan data pengguna dan riwayat login ke file."""
    with open(USER_DB, "wb") as file:
        pickle.dump(users, file)
    with open(LOGIN_HISTORY, "wb") as file:
        pickle.dump(login_history, file)

def register():
    """Fungsi untuk registrasi pengguna baru."""
    username = input("Masukkan username: ")
    if username in users:
        print("Username sudah terdaftar.")
        return
    password = input("Masukkan password: ")
    users[username] = {"password": password, "group_members": []}
    save_data()
    print("Registrasi berhasil.")

def login():
    """Fungsi untuk login pengguna."""
    username = input("Masukkan username: ")
    if username not in users:
        print("Username tidak ditemukan.")
        return None
    password = input("Masukkan password: ")
    if users[username]["password"] == password:
        print("Login berhasil.")
        login_history.append({"username": username, "timestamp": datetime.now()})
        save_data()
        return username
    else:
        print("Password salah.")
        return None

def add_group_member(username):
    """Fungsi untuk menambahkan anggota kelompok."""
    if username is None:
        print("Anda harus login terlebih dahulu.")
        return
    member_name = input("Masukkan nama anggota kelompok baru: ")
    users[username]["group_members"].append(member_name)
    save_data()
    print(f"{member_name} berhasil ditambahkan ke kelompok.")

def view_login_history():
    """Fungsi untuk melihat riwayat login."""
    print("\nRiwayat Login:")
    for record in login_history:
        print(f"{record['username']} - {record['timestamp']}")

def main():
    """Menu utama aplikasi."""
    while True:
        print("\nMenu:")
        print("1. Registrasi")
        print("2. Login")
        print("3. Lihat Riwayat Login")
        print("4. Keluar")
        choice = input("Pilih menu: ")

        if choice == "1":
            register()
        elif choice == "2":
            username = login()
            if username:
                while True:
                    print("\nMenu Setelah Login:")
                    print("1. Tambah Anggota Kelompok")
                    print("2. Logout")
                    sub_choice = input("Pilih menu: ")

                    if sub_choice == "1":
                        add_group_member(username)
                    elif sub_choice == "2":
                        print("Logout berhasil.")
                        break
                    else:
                        print("Pilihan tidak valid.")
        elif choice == "3":
            view_login_history()
        elif choice == "4":
            print("Keluar dari program.")
            break
        else:
            print("Pilihan tidak valid.")

if __name__ == "__main__":
    main()
