# DRIS Calculation Script

def calculate_dris_index(sample_ratios, norm_ratios, cv_ratios):
    dris_functions = {}
    
    # Compute f(A/B) for each nutrient ratio
    for key in sample_ratios:
        f_value = ((sample_ratios[key] - norm_ratios[key]) / cv_ratios[key]) * 100
        dris_functions[key] = f_value
    
    # Compute DRIS Index for each nutrient
    dris_indices = {}
    nutrient_counts = {}
    
    for key, value in dris_functions.items():
        nutrient_a, nutrient_b = key.split('/')
        
        for nutrient in [nutrient_a, nutrient_b]:
            if nutrient not in dris_indices:
                dris_indices[nutrient] = 0
                nutrient_counts[nutrient] = 0
            
            dris_indices[nutrient] += value
            nutrient_counts[nutrient] += 1
    
    # Average the indices
    for nutrient in dris_indices:
        dris_indices[nutrient] /= nutrient_counts[nutrient]
    
    return dris_indices

# Sample Data
sample_concentrations = {'N': 3.5, 'P': 0.3, 'K': 2.5}
norm_ratios = {'N/P': 12.0, 'N/K': 1.5}
cv_ratios = {'N/P': 10, 'N/K': 12}

# Compute Sample Ratios
sample_ratios = {
    'N/P': sample_concentrations['N'] / sample_concentrations['P'],
    'N/K': sample_concentrations['N'] / sample_concentrations['K']
}

# Compute DRIS Indices
dris_indices = calculate_dris_index(sample_ratios, norm_ratios, cv_ratios)

# Output Results
print("DRIS Indices:")
for nutrient, index in dris_indices.items():
    print(f"{nutrient}: {index:.2f}")

