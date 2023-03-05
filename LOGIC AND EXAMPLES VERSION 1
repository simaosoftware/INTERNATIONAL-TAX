# DEFINING TAX SYSTEMS LOGIC
def progressive_tax_system(tax_rates, deductible_rates, deduct_expenses, average_salary):
    taxes = 0
    if deduct_expenses.lower() == "n" or deduct_expenses.lower() == "no" or deduct_expenses is None:
        for rate, limit in tax_rates:
            if average_salary > limit:
                taxes += limit * rate
                average_salary -= limit
            else:
                taxes += average_salary * rate
                break
        return taxes
        
    elif deduct_expenses.lower() == "y" or deduct_expenses.lower() == "yes":
        for rate, limit in tax_rates:
            average_salary = average_salary * deductible_rates[0]
            if average_salary > limit:
                taxes += limit * rate
                average_salary -= limit
            else:
                taxes += average_salary * rate
                break
        return taxes

def ss_system(ss_rates, deductible_rates, deduct_expenses, type_of_worker):
    ss = 0
    if type_of_worker.lower() == "employee":
        ss += average_salary * ss_rates[0]
    elif type_of_worker.lower() == "contractor" and (deduct_expenses.lower() == "no" or deduct_expenses.lower() == "n"):
        ss += average_salary * ss_rates[1]
    elif type_of_worker.lower() == "contractor" and (deduct_expenses.lower() == "y" or deduct_expenses.lower() == "yes"):
            ss += average_salary * deductible_rates[1] * ss_rates[1]
    return ss

# LOCAL GOVERNMENT NUMBERS
def us_taxes_and_ss(average_salary, type_of_worker, deduct_expenses):
    tax_rates = [(0.1, 10000), (0.12, 40000), (0.22, 90000), (0.24, 170000), (0.32, 215000), (0.35, 320000), (0.37, float("inf"))]
    ss_rates = [0.07, 0.15] # [employee, freelancer]
    deductible_rates = [0.75, 0.7] # [taxes, social security]
    
    taxes = progressive_tax_system(tax_rates, deductible_rates, deduct_expenses, average_salary)
    ss = ss_system(ss_rates, deductible_rates, deduct_expenses, type_of_worker)
    return taxes, ss


def portugal_taxes_and_ss(average_salary, type_of_worker, deduct_expenses):
    tax_rates = [(0.145, 7500), (0.21, 11300), (0.26, 16000), (0.285, 20700), (0.35, 26350), (0.37, 38700), (0.435, 50500), (0.45, 78850), (0.48, float("inf"))]
    ss_rates = [0.11, 0.7*0.21, 0.21]
    deductible_rates = [0.75, 0.7]
    
    taxes = progressive_tax_system(tax_rates, deductible_rates, deduct_expenses, average_salary)
    ss = ss_system(ss_rates, deductible_rates, deduct_expenses, type_of_worker)
    return taxes, ss

# MAPPING OF ALLOWED COUNTRIES TO CORRESPONDING TAX FUNCTIONS
allowed_countries = {
    "United States": us_taxes_and_ss, 
    "Portugal": portugal_taxes_and_ss
}

# USER INPUTS FOR V1 (EMPLOYEE OR CONTRACTOR WITH NO COMPANIES AND NO INVESTMENTS LOOKING TO MOVE ABROAD)
while True:
    type_of_worker = input(f"What type of worker are you? ")
    if type_of_worker.lower() == "contractor":
        deduct_expenses = input("Can you deduct pre-tax expenses ? (yes or no) ")
        if deduct_expenses.lower() == "y" or deduct_expenses.lower() == "yes":
            break
        elif deduct_expenses.lower() == "n" or deduct_expenses.lower() == "no":
            break
        else:
            print("Invalid answer. Choose yes or no " )
    if type_of_worker.lower() == "employee":
        deduct_expenses = "no"
        break
    else:
        print("Invalid employment type. Choose employee or contractor " )

while True:
    average_salary = float(input(f"What is your average salary/year? "))
    if average_salary >= 5000:
        break
    else:
        print("Invalid. Minimum is 5000?/year quantity of a given currency unit ")

while True:
    main_tax_residency = input(f"Current tax resident of country? ({', '.join(allowed_countries.keys())}): ")
    if main_tax_residency in allowed_countries:
        break
    else:
        print("Invalid country. Please choose from the list of allowed countries. ")

# Calculate taxes paid to the selected country
tax_function = allowed_countries[main_tax_residency]
taxes, ss = tax_function(average_salary, type_of_worker, deduct_expenses)

# Printing all the inputs provided by the user
print(f"Your local government pockets: {taxes} per year")
print(f"Mismanaged pension funds get: {ss}")
