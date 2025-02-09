import pygame
import pickle

# Ses modülü başlatılıyor
pygame.mixer.init()

# Desteklenen diller ve dosya yolları
LANGUAGES = {
    "English": {"voice": "voices/english/", "subtitle": "subtitles/english.txt"},
    "Turkish": {"voice": "voices/turkish/", "subtitle": "subtitles/turkish.txt"},
    "Spanish": {"voice": "voices/spanish/", "subtitle": "subtitles/spanish.txt"}
}

# NPC Sınıfı
class NPC:
    def __init__(self, name, mission):
        self.name = name
        self.mission = mission

    def give_mission(self):
        print(f"{self.name} gives you a mission: {self.mission}")
        return self.mission

# Görev Sınıfı
class Mission:
    def __init__(self, name, description, reward, is_main_mission=False):
        self.name = name
        self.description = description
        self.reward = reward
        self.is_main_mission = is_main_mission
        self.completed = False

    def complete_mission(self):
        if not self.completed:
            self.completed = True
            print(f"Mission '{self.name}' completed! You earned ${self.reward}.")
            return self.reward
        else:
            print(f"Mission '{self.name}' already completed.")
            return 0

# Oyun Sınıfı
class Game:
    def __init__(self, player_name, difficulty, language="English"):
        self.player_name = player_name
        self.difficulty = difficulty
        self.language = language
        self.money = 1500
        self.inventory = []
        self.current_city = None
        self.game_running = True
        self.city_list = ["Agricultural Town", "Forest Village", "Desert Oasis", "Mountain Retreat"]

        # Görevler (Ana ve Yan Görevler)
        self.missions = [
            Mission("Rescue the Mayor", "Rescue the kidnapped mayor.", 1000, is_main_mission=True),
            Mission("Bank Heist", "Help your ally rob a bank.", 1500),
            Mission("Retrieve Artifact", "Find the lost artifact in the forest.", 500),
            Mission("Defeat William Steak", "Defeat the main antagonist, William Steak.", 2000, is_main_mission=True)
        ]

        # Her şehir için dükkanlar
        self.shops = {
            "Agricultural Town": [("Cheap Fish", 150), ("Medium Fish", 300), ("Expensive Fish", 500)],
            "Forest Village": [("Cheap Meat", 200), ("Medium Meat", 400), ("Expensive Meat", 600)],
            "Desert Oasis": [("Cheap Weapon", 250), ("Medium Weapon", 500), ("Expensive Weapon", 750)],
            "Mountain Retreat": [("Cheap Clothing", 100), ("Medium Clothing", 200), ("Expensive Clothing", 350)]
        }

        # Her şehirde oteller (Ucuz, Orta, Pahalı)
        self.hotels = {
            "Agricultural Town": {"Cheap Hotel": 100, "Medium Hotel": 200, "Expensive Hotel": 500},
            "Forest Village": {"Cheap Inn": 120, "Comfort Lodge": 250, "Luxury Suites": 600},
            "Desert Oasis": {"Simple Tent": 80, "Oasis Hotel": 180, "Royal Palace": 700},
            "Mountain Retreat": {"Cabin Stay": 150, "Mountain Lodge": 300, "Resort Villa": 750}
        }

        # NPC'ler
        self.npc_list = {
            "Agricultural Town": [NPC("Farmer Joe", "Harvest the crops."), NPC("Lily", "Guide the flock of sheep.")],
            "Forest Village": [NPC("Druidess", "Find the enchanted tree."), NPC("Hunter Ragnar", "Protect the forest.")],
            "Desert Oasis": [NPC("Sahar", "Retrieve water from the oasis."), NPC("Nomad Kadir", "Guide the caravan.")],
            "Mountain Retreat": [NPC("Bjorn", "Find the frozen relic."), NPC("Ingrid", "Defend the mountain village.")]
        }

        self.completed_missions = []
        self.checkpoints = []

    # Oyun Başlatma
    def start_game(self):
        print(f"Welcome to Drake's Revenge, {self.player_name}!")
        print("The adventure begins now!")

    # Şehir Seçimi
    def choose_city(self):
        print("Available cities:")
        for city in self.city_list:
            print(f"- {city}")
        city_choice = input("Choose a city to explore: ")
        if city_choice in self.city_list:
            self.current_city = city_choice
            print(f"You have arrived in {city_choice}.")
        else:
            print("Invalid city choice.")

    # Dükkan Ziyareti
    def visit_shops(self):
        if self.current_city:
            print(f"Shops in {self.current_city}:")
            for item, price in self.shops[self.current_city]:
                print(f"{item} - ${price}")

            item_choice = input("Choose an item to buy: ")
            for item, price in self.shops[self.current_city]:
                if item_choice == item and self.money >= price:
                    self.money -= price
                    self.inventory.append(item)
                    print(f"Purchased {item}. Remaining money: ${self.money}")
                    return
            print("Insufficient funds or item does not exist.")
        else:
            print("Choose a city first.")

    # Otel Ziyareti
    def visit_hotels(self):
        if self.current_city:
            print(f"Hotels in {self.current_city}:")
            for hotel, price in self.hotels[self.current_city].items():
                print(f"{hotel}: ${price}")

            hotel_choice = input("Choose a hotel: ")
            if hotel_choice in self.hotels[self.current_city]:
                price = self.hotels[self.current_city][hotel_choice]
                if self.money >= price:
                    self.money -= price
                    print(f"Stayed at {hotel_choice}. Health restored. Remaining money: ${self.money}")
                else:
                    print("Not enough money to stay here.")
            else:
                print("Invalid hotel choice.")
        else:
            print("Choose a city first.")

    # Görev Al
    def take_mission(self):
        print("Available Missions:")
        for mission in self.missions:
            if not mission.completed:
                print(f"- {mission.name}: {mission.description}")

        mission_choice = input("Choose a mission: ")
        selected_mission = next((m for m in self.missions if m.name.lower() == mission_choice.lower()), None)

        if selected_mission:
            selected_mission.complete_mission()
            self.money += selected_mission.reward
        else:
            print("Invalid mission choice.")

    # Envanteri Görüntüle
    def check_inventory(self):
        if self.inventory:
            print("Inventory:", ", ".join(self.inventory))
        else:
            print("Inventory is empty.")

    # Oyun Kaydetme
    def save_game(self):
        with open(f"{self.player_name}_game_save.pickle", "wb") as save_file:
            pickle.dump(self, save_file)
        print("Game saved successfully.")

    # Oyun Yükleme
    @staticmethod
    def load_game(player_name):
        try:
            with open(f"{player_name}_game_save.pickle", "rb") as save_file:
                game = pickle.load(save_file)
            print("Game loaded successfully.")
            return game
        except FileNotFoundError:
            print("No saved game found.")
            return None

# Oyunu Başlat
player_name = input("Enter your player name: ")
difficulty = input("Choose difficulty (Easy, Normal, Hard): ")
language = input("Choose language (English, Turkish, Spanish): ")

game = Game(player_name, difficulty, language)
game.start_game()

# Şehir Seçimi ve Etkileşim
while game.game_running:
    print("\nOptions: [1] Choose City [2] Visit Shop [3] Visit Hotel [4] Take Mission [5] Inventory [6] Save [7] Load [8] Exit")
    choice = input("Choose an option: ")

    if choice == "1":
        game.choose_city()
    elif choice == "2":
        game.visit_shops()
    elif choice == "3":
        game.visit_hotels()
    elif choice == "4":
        game.take_mission()
    elif choice == "5":
        game.check_inventory()
    elif choice == "6":
        game.save_game()
    elif choice == "7":
        loaded_game = Game.load_game(player_name)
        if loaded_game


