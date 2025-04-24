# Footballsimulator
A Python-based object‑oriented simulator to test offensive and defensive football strategies.   Demonstrates encapsulation, inheritance, polymorphism, and abstraction with 12+ classes.

# Key features
In this project, the code makes use of:

Encapsulation: Classes like Player, Team, and Scoreboard encapsulate data (attributes) and behavior (methods) together.

Inheritance: An abstract base class Player is extended by six concrete player types (Quarterback, RunningBack, WideReceiver, Linebacker, Cornerback, Kicker).

Polymorphism: Each Player subclass implements its own make_play() logic, allowing the simulator to treat all players uniformly when summing performance. Similarly, different Strategy objects can be interchanged at runtime.

Abstraction: The Player class is defined as an abstract base class (using Python’s abc module), exposing only the make_play() and rest() interfaces to consumers.

Randomized Strategy Selection: Each Coach holds multiple Strategy objects and randomly picks one per play.

Turnover‑Based Possession: Possession switches only when an offensive play fails.

Variable Scoring: Successful plays award a random number of points chosen from a set (2, 3, 6, 7, or 8).

Fatigue Simulation: Player stamina decreases after each play, affecting future performance.

# Plant UML
//www.plantuml.com/plantuml/dpng/XPGnRzim48Lt_Gh2IvpM7heAGP2sGuUsG9iKw4YSgBDDc2857fd6QFlVSo8bcHOCPBBqZkBxxXtf1mhWG-nCTIC-DXGyg20Q81JA0545evPVnGy39_JYRoN4LbZei1PSJs-yKQSjr4BRa8MZDaOQV4OpYUz51qUKFU-olatl7Ydmu_-A_Jyxgpm657rseTWaLwGgk_-Cp8g-0NKSbSTehRRxJsVbjsxn4HNgP_IZz4rR7BwP13RozcARhpfLgx6zkt_RRMbMwgLbgLyObuNIlf1BY5AUK6vIrW9iAvw2Xu3xKtMUmRK9fDXaEFM5v3KTBqUmJTEL59L28ZdiN8kTSP3dWo-eOi2rs-raZtwt4ItcfzjQDCuP7bNccB565Wediu3XzSUQ-wTmDq3V9mT2WogCPw1EtGnxphkjFgo4uo1MxrQfc4y6g4FAuXT2b_spU3K8V65WZLRXidVABXffv9fiCviwE4UuPCj-oVggdHf0wimpzuwUYjRHPmDeMvchw_oJRy4UWmGL-EYag9tlWE4PePZC3eBUHyoN9R4iaqA_qFatGt64JyfvR1puDfyftKK3mOrNeYoFunm4Rk4xz4D8VE8t7E9zgWNTeVJOWRQidoztx5i8ADEdwy0eSWXg8th7AoYtschAyPw3_SABPfxHdiyVMi8wfPwK5fSApSnrmJ-N64swFcXdfqamnNW6_-YW3h9tH4Z4AwP_esZbdcdMDdPfJfIQIwoNb6N6AVHwGXRYxQKjlhcqxEW1RHixyme0
# Code
# football_simulator.py

from abc import ABC, abstractmethod
import random

# ─── Players ────────────────────────────────────────────────────────────────

class Player(ABC):
    def __init__(self, name, stamina, skill_level):
        self.name = name
        self.stamina = stamina
        self.skill_level = skill_level

    @abstractmethod
    def make_play(self):
        pass

    def rest(self):
        self.stamina = min(100, self.stamina + 10)

class Quarterback(Player):
    def make_play(self):
        return self.skill_level * (1.2 if self.stamina > 30 else 0.6)

class RunningBack(Player):
    def make_play(self):
        return self.skill_level * (1.1 if self.stamina > 40 else 0.7)

class WideReceiver(Player):
    def make_play(self):
        return self.skill_level * (1.3 if self.stamina > 50 else 0.5)

class Linebacker(Player):
    def make_play(self):
        return self.skill_level * (1.0 if self.stamina > 35 else 0.8)

class Cornerback(Player):
    def make_play(self):
        return self.skill_level * (1.05 if self.stamina > 40 else 0.75)

class Kicker(Player):
    def make_play(self):
        return self.skill_level * (1.5 if self.stamina > 20 else 0.6)


# Play & Strategy

class Play:
    def __init__(self, name, play_type, risk_level, base_success_rate):
        self.name = name
        self.play_type = play_type      # "offensive" or "defensive"
        self.risk_level = risk_level    # 1–5
        self.base_success_rate = base_success_rate  # 0.0–1.0

    def execute(self, offense_perf, defense_perf):
        adjusted = self.base_success_rate + (offense_perf - defense_perf) * 0.005
        adjusted -= self.risk_level * 0.02
        return random.random() < min(max(adjusted, 0), 1)

class Strategy:
    def __init__(self, style, playbook):
        self.style = style
        self.playbook = playbook

    def select_play(self):
        return random.choice(self.playbook)


# Team & Coach 

class Coach:
    def __init__(self, name, experience_level, strategies):
        self.name = name
        self.experience_level = experience_level
        self.strategies = strategies

    def choose_play(self):
        strat = random.choice(self.strategies)
        return strat.select_play()

class Team:
    def __init__(self, name, coach):
        self.name = name
        self.coach = coach
        self.players = []

    def add_player(self, player):
        self.players.append(player)

    def get_active_performance(self):
        return sum(p.make_play() for p in self.players if p.stamina > 20)

    def fatigue(self):
        for p in self.players:
            p.stamina = max(0, p.stamina - 5)


# Scoreboard & Simulator 

class Scoreboard:
    def __init__(self, team1, team2):
        self.scores = {team1.name: 0, team2.name: 0}
        self.time_remaining = 60

    def update_score(self, team_name, points):
        self.scores[team_name] += points

    def display(self):
        print("\n=== Scoreboard ===")
        for t, s in self.scores.items():
            print(f"{t}: {s}")
        print(f"Time Remaining: {self.time_remaining} mins\n")


class GameSimulator:
    def __init__(self, team1, team2, scoreboard):
        self.offense, self.defense = team1, team2
        self.scoreboard = scoreboard
        self.scoring_options = [2, 3, 6, 7, 8]

    def switch_possession(self):
        self.offense, self.defense = self.defense, self.offense

    def run_game(self, num_plays=6):
        for i in range(1, num_plays + 1):
            print(f"--- Play {i} ---")
            play = self.offense.coach.choose_play()
            off_perf = self.offense.get_active_performance()
            def_perf = self.defense.get_active_performance()

            success = play.execute(off_perf, def_perf)
            print(f"{self.offense.name} calls {play.name} -> {'SUCCESS' if success else 'FAIL'}")

            if success:
                pts = random.choice(self.scoring_options)
                print(f"{self.offense.name} scores {pts} points!")
                self.scoreboard.update_score(self.offense.name, pts)
                # offense retains the ball
            else:
                print(f"Turnover! {self.defense.name} takes over.")
                self.switch_possession()

            # apply fatigue and time
            self.offense.fatigue()
            self.defense.fatigue()
            self.scoreboard.time_remaining -= 10
            self.scoreboard.display()


# Main Demo 

if __name__ == "__main__":
    off_plays = [Play("Long Pass", "offensive", 3, 0.5),
                 Play("Run Play", "offensive", 1, 0.7)]
    def_plays = [Play("Zone Defense", "defensive", 2, 0.6),
                 Play("Blitz", "defensive", 3, 0.4)]

    team1_strat = [Strategy("agg", off_plays), Strategy("bal", def_plays)]
    team2_strat = [Strategy("cons", off_plays), Strategy("agg", def_plays)]

    coach1 = Coach("Coach A", 8, team1_strat)
    coach2 = Coach("Coach B", 9, team2_strat)
    team1 = Team("Sharks", coach1)
    team2 = Team("Tigers", coach2)

    for idx in range(2):
        team1.add_player(Quarterback(f"QB{idx}", 100, 80))
        team1.add_player(RunningBack(f"RB{idx}", 90, 75))
        team2.add_player(WideReceiver(f"WR{idx}", 95, 78))
        team2.add_player(Linebacker(f"LB{idx}", 85, 70))
    team1.add_player(Kicker("Kicker1", 90, 60))
    team2.add_player(Cornerback("CB1", 88, 65))

    board = Scoreboard(team1, team2)
    sim = GameSimulator(team1, team2, board)
    sim.run_game(num_plays=6)
