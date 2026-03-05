GST_RATE = 0.05


class BurgerOrderingSystem:

    def __init__(self):
        self.menu = {
            "Burger": 120,
            "Fries": 80,
            "Coke": 50,
            "Pizza": 200,
            "Wrap": 150
        }
        self.cart = {}

    def display_menu(self):
        print("\n========== MENU ==========")
        for index, (item, price) in enumerate(self.menu.items(), start=1):
            print(f"{index}. {item:<10} : ₹{price}")
        print("==========================")

    def add_item(self):
        item = input("Enter item name: ").title()

        if item not in self.menu:
            print("Item not available.")
            return

        try:
            quantity = int(input("Enter quantity: "))
            if quantity <= 0:
                print("Quantity must be greater than zero.")
                return
        except ValueError:
            print("Invalid quantity.")
            return

        if item in self.cart:
            self.cart[item] += quantity
        else:
            self.cart[item] = quantity

        print(f"{item} x{quantity} added successfully.")

    def remove_item(self):
        if not self.cart:
            print("Cart is empty.")
            return

        item = input("Enter item to remove: ").title()

        if item in self.cart:
            del self.cart[item]
            print(f"{item} removed from cart.")
        else:
            print("Item not found in cart.")

    def view_cart(self):
        if not self.cart:
            print("Cart is empty.")
            return

        print("\n========== YOUR CART ==========")
        total = 0
        for item, quantity in self.cart.items():
            cost = self.menu[item] * quantity
            total += cost
            print(f"{item:<10} x{quantity} = ₹{cost}")

        print("--------------------------------")
        print(f"Subtotal: ₹{total}")
        print("================================")

    def calculate_bill(self):
        subtotal = sum(self.menu[item] * qty for item, qty in self.cart.items())
        gst = subtotal * GST_RATE
        total = subtotal + gst
        return subtotal, gst, total

    def print_bill(self):
        if not self.cart:
            print("Cart is empty.")
            return

        print("\n=========== FINAL BILL ===========")
        subtotal, gst, total = self.calculate_bill()

        for item, quantity in self.cart.items():
            print(f"{item:<10} x{quantity} = ₹{self.menu[item] * quantity}")

        print("----------------------------------")
        print(f"Subtotal     : ₹{subtotal:.2f}")
        print(f"GST (5%)     : ₹{gst:.2f}")
        print(f"Grand Total  : ₹{total:.2f}")
        print("==================================")

    def clear_cart(self):
        self.cart.clear()
        print("Cart cleared successfully.")

    def run(self):
        while True:
            print("\n=========== ORDER SYSTEM ===========")
            print("1. View Menu")
            print("2. Add Item")
            print("3. Remove Item")
            print("4. View Cart")
            print("5. Print Bill")
            print("6. Clear Cart")
            print("7. Exit")
            print("====================================")

            choice = input("Enter your choice: ")

            if choice == "1":
                self.display_menu()
            elif choice == "2":
                self.add_item()
            elif choice == "3":
                self.remove_item()
            elif choice == "4":
                self.view_cart()
            elif choice == "5":
                self.print_bill()
            elif choice == "6":
                self.clear_cart()
            elif choice == "7":
                print("Thank you for using Burger Ordering System.")
                break
            else:
                print("Invalid choice. Please try again.")


if __name__ == "__main__":
    system = BurgerOrderingSystem()
    system.run()
