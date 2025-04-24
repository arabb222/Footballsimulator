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
