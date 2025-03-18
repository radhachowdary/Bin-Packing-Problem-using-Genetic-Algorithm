//The Bin Packing Problem (BPP) is a combinatorial optimization problem where items of varying sizes must be packed into a minimum number of bins, each with a fixed capacity. The objective is to distribute items efficiently to reduce waste while ensuring that no bin exceeds its capacity.
import random
BIN_CAPACITY = 10
ITEMS = [5, 7, 8, 3, 6, 2, 9]  
POPULATION_SIZE = 50
GENERATIONS = 200
MUTATION_RATE = 0.1
def generate_initial_population():
    population = []
    for _ in range(POPULATION_SIZE):
        solution = [random.randint(0, len(ITEMS) - 1) for _ in ITEMS] 
        population.append(solution)
    return population
def fitness(solution):
    bins = {}
    for i, bin_index in enumerate(solution):
        bins.setdefault(bin_index, 0)
        bins[bin_index] += ITEMS[i]
    num_bins = len(bins)
    penalty = sum(max(0, bins[b] - BIN_CAPACITY) for b in bins)  
    return num_bins + penalty  
def selection(population):
    return min(random.sample(population, 5), key=fitness)  
def crossover(parent1, parent2):
    point = random.randint(1, len(ITEMS) - 1)
    child1 = parent1[:point] + parent2[point:]
    child2 = parent2[:point] + parent1[point:]
    return child1, child2
def mutate(solution):
    if random.random() < MUTATION_RATE:
        index = random.randint(0, len(solution) - 1)
        solution[index] = random.randint(0, len(ITEMS) - 1)
    return solution
def genetic_algorithm():
    population = generate_initial_population()
    for _ in range(GENERATIONS):
        population = sorted(population, key=fitness)[:POPULATION_SIZE]  # Keep best solutions
        next_generation = []
        while len(next_generation) < POPULATION_SIZE:
            parent1, parent2 = selection(population), selection(population)
            child1, child2 = crossover(parent1, parent2)
            next_generation.extend([mutate(child1), mutate(child2)])
        population = next_generation[:POPULATION_SIZE]
    best_solution = min(population, key=fitness)
    return best_solution, fitness(best_solution)
best_solution, best_fitness = genetic_algorithm()
print(f"Optimal Bin Packing: {best_solution}")
print(f"Minimum Number of Bins Used: {best_fitness}")


