receipt_number = 100000
transaction_id = 1


accounts = [
    ["lorence", "1234", 5000],
    ["rod", "5678", 3000],
    ["klint", "2345", 2000],
    ["cloe", "4321", 2300],
]


transactions = []


while True:
    print("\n" + "="*50)
    print("        GROUP 9 E WALLET SYSTEM")
    print("="*50)
    print("Type 'logout' at any prompt to exit")
    
    username = input("\nUsername: ")
    if username == "logout":
        print("Goodbye!")
        break
    
    pin = input("PIN: ")
    if pin == "logout":
        print("Goodbye!")
        break
    
    
    user_pos = -1
    i = 0
    while i < len(accounts):
        if accounts[i][0] == username and accounts[i][1] == pin:
            user_pos = i
            break
        i = i + 1
    
    if user_pos == -1:
        print("\nWrong username or PIN.")
        continue
 
    
    while True:
        print("\n" + "="*50)
        print("Welcome " + username)
        print("Balance: P" + str(accounts[user_pos][2]))
        print("="*50)
        print("Commands: SEND | BALANCE | HISTORY | CASHOUT | LOGOUT")
        
        command = input("\n> ")
        
        
        if command == "BALANCE":
            print("\nYour balance: P" + str(accounts[user_pos][2]))
            input("Press Enter to continue...")
        
        
        elif command == "HISTORY":
            while True:
                print("\n--- YOUR TRANSACTIONS ---")
                
                
                my_txns = []
                i = 0
                while i < len(transactions):
                    txn = transactions[i]
                    if txn[2] == username or txn[3] == username:
                        my_txns.append(txn)
                    i = i + 1
                
                
                if len(my_txns) == 0:
                    print("No transactions found.")
                else:
                    print("\n#   Type   Amount   Receipt")
                    print("-" * 35)
                    i = 0
                    while i < len(my_txns):
                        txn = my_txns[i]
                        if txn[2] == username:
                            direction = "OUT"
                        elif txn[3] == "CASHOUT":
                            direction = "CASH"
                        else:
                            direction = "IN"
                        print(str(i+1) + "   " + direction + "    P" + str(txn[4]) + "     " + str(txn[0]))
                        i = i + 1
                
                
                choice = input("\nType 'back' or 'view <number>': ")
                
                if choice == "back":
                    break
                
                if len(choice) >= 5 and choice[0:4] == "view":
                    try:
                        num = int(choice[5:])
                        if num >= 1 and num <= len(my_txns):
                            t = my_txns[num-1]
                            print("\n" + "="*50)
                            print("TRANSACTION DETAILS")
                            print("="*50)
                            print("Receipt #: " + str(t[0]))
                            print("Txn ID: " + str(t[1]))
                            if t[3] == "CASHOUT":
                                print("Type: CASHOUT")
                                print("Account: " + t[2])
                            else:
                                print("Sender: " + t[2])
                                print("Receiver: " + t[3])
                            print("Amount: P" + str(t[4]))
                            print("-"*50)
                            if t[3] == "CASHOUT":
                                print("Remaining Balance: P" + str(t[5]))
                            else:
                                print("Sender Balance: P" + str(t[5]))
                                print("Receiver Balance: P" + str(t[6]))
                            print("="*50)
                            input("Press Enter to continue...")
                        else:
                            print("Invalid transaction number.")
                    except:
                        print("Please enter a valid number.")
                else:
                    print("Invalid command.")
        
        
        elif command == "SEND":
            while True:
                print("\n--- SEND MONEY ---")
                
                to_user = input("Send to username (or 'back'): ")
                if to_user == "back":
                    break
                
                if to_user == username:
                    print("You cannot send money to yourself.")
                    continue
                
                
                to_pos = -1
                i = 0
                while i < len(accounts):
                    if accounts[i][0] == to_user:
                        to_pos = i
                        break
                    i = i + 1
                
                if to_pos == -1:
                    print("User not found.")
                    if input("Try again? (yes/back): ") != "yes":
                        break
                    continue
                
                
                amount_str = input("Amount (or 'back'): ")
                if amount_str == "back":
                    continue
                
                
                try:
                    amount = float(amount_str)
                except:
                    print("Invalid amount. Must be a number.")
                    if input("Try again? (yes/back): ") != "yes":
                        break
                    continue
                
                if amount <= 0:
                    print("Amount must be greater than zero.")
                    if input("Try again? (yes/back): ") != "yes":
                        break
                    continue
                
                if accounts[user_pos][2] < amount:
                    print("Insufficient funds. Your balance: P" + str(accounts[user_pos][2]))
                    if input("Try again? (yes/back): ") != "yes":
                        break
                    continue
                
                
                print("\nTransfer P" + str(amount) + " to " + to_user + "?")
                confirm = input("Type 'confirm' to proceed: ")
                if confirm != "confirm":
                    print("Transaction cancelled.")
                    break
                
                
                accounts[user_pos][2] = accounts[user_pos][2] - amount
                accounts[to_pos][2] = accounts[to_pos][2] + amount
                
                # Update counters
                receipt_number = receipt_number + 1
                current_id = transaction_id
                transaction_id = transaction_id + 1
                
                
                new_txn = [receipt_number, current_id, username, to_user, amount, accounts[user_pos][2], accounts[to_pos][2]]
                transactions.append(new_txn)
                
                
                print("\n" + "="*50)
                print("TRANSFER SUCCESSFUL!")
                print("="*50)
                print("Receipt #: " + str(receipt_number))
                print("Txn ID: " + str(current_id))
                print("Amount: P" + str(amount))
                print("Recipient: " + to_user)
                print("Your New Balance: P" + str(accounts[user_pos][2]))
                print("="*50)
                
                again = input("\nSend again? (yes/back): ")
                if again != "yes":
                    break
        
        
        elif command == "CASHOUT":
            while True:
                print("\n--- CASHOUT MONEY ---")
                
                amount_str = input("Amount to cashout (or 'back'): ")
                if amount_str == "back":
                    break
                
                try:
                    amount = float(amount_str)
                except:
                    print("Invalid amount. Must be a number.")
                    if input("Try again? (yes/back): ") != "yes":
                        break
                    continue
                
                if amount <= 0:
                    print("Amount must be greater than zero.")
                    if input("Try again? (yes/back): ") != "yes":
                        break
                    continue
                
                if accounts[user_pos][2] < amount:
                    print("Insufficient funds. Your balance: P" + str(accounts[user_pos][2]))
                    if input("Try again? (yes/back): ") != "yes":
                        break
                    continue
                
                print("\nCashout P" + str(amount) + " from your account?")
                confirm = input("Type 'confirm' to proceed: ")
                if confirm != "confirm":
                    print("Cashout cancelled.")
                    break
                
                
                accounts[user_pos][2] = accounts[user_pos][2] - amount
                
                
                receipt_number = receipt_number + 1
                current_id = transaction_id
                transaction_id = transaction_id + 1
                
                
                new_txn = [receipt_number, current_id, username, "CASHOUT", amount, accounts[user_pos][2], 0]
                transactions.append(new_txn)
                
                
                print("\n" + "="*50)
                print("CASHOUT SUCCESSFUL!")
                print("="*50)
                print("Receipt #: " + str(receipt_number))
                print("Txn ID: " + str(current_id))
                print("Amount: P" + str(amount))
                print("Your New Balance: P" + str(accounts[user_pos][2]))
                print("="*50)
                
                again = input("\nCashout again? (yes/back): ")
                if again != "yes":
                    break
        
        
        elif command == "LOGOUT":
            print("\nLogging out...")
            break
        
        
        else:
            print("\nUnknown command. Use: SEND, BALANCE, HISTORY, CASHOUT, LOGOUT")
