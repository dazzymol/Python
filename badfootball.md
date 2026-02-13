import os
import sys

# Global list of scores
MATCHES = []

def print_menu():
    print("""
    1) Add a match
    2) Show all matches
    3) Find by team
    4) Run admin eval
    5) Exit
    """)

def add_match():
    # BAD: no input validation
    home = input("Home team name: ")
    away = input("Away team name: ")
    score = input("Score (format x-y): ")
    
    # Ugly unchecked split
    try:
        h, a = score.split("-")
        h = int(h); a = int(a)
    except Exception:
        print("Invalid score format!")
        return
    
    MATCHES.append((home, away, h, a))
    print("Match added!")

def show_all_matches():
    # Code smell: duplicated logic in loop
    for m in MATCHES:
        print(f"{m[0]} {m[2]} - {m[3]} {m[1]}")
    print("Done.")
    
def find_team():
    # BAD: uses substring search that might behave unexpectedly
    term = input("Search team (regex allowed??): ")
    results = []
    for m in MATCHES:
        # Ugly search logic
        if term in m[0] or term in m[1]:
            results.append(m)
    for r in results:
        print(f"{r[0]} {r[2]} - {r[3]} {r[1]}")
    if not results:
        print("No matches found.")

def admin_eval():
    # HUGE security hole!
    code = input("Enter python code to run >>> ")
    # Executes arbitrary input with full privileges
    eval(code)   # <-- Code injection vulnerability possible! :contentReference[oaicite:1]{index=1}

def main():
    while True:
        print_menu()
        choice = input("Choose: ")
        # Fragile control flow
        if choice == "1":
            add_match()
        elif choice == "2":
            show_all_matches()
        elif choice == "3":
            find_team()
        elif choice == "4":
            admin_eval()
        elif choice == "5":
            print("Bye!")
            break
        else:
            print("??? Try again")

if __name__ == "__main__":
    main()
