#include <stdio.h>
#include <stdlib.h>
#include <time.h>

const int NUM_PAWNS    = 5; 
const int NUM_COLORS   = 8; 
const int NUM_ATTEMPTS = 10;

enum color_t { RED, GREEN, BLUE, YELLOW, BLACK, WHITE, GRAY, PURPLE }; 

typedef enum color_t board[NUM_PAWNS];

void generate_hidden_combination(enum color_t hidden_combination[]) {
    srand(time(NULL));
    for (int i = 0; i < NUM_PAWNS; ++i) {
        hidden_combination[i] = rand() % NUM_COLORS;
    }
}

void read_proposed_combination(enum color_t board[]) {
    printf("Enter your proposed combination (0-7 for colors):\n");
    for (int i = 0; i < NUM_PAWNS; ++i) {
        printf("Pawn %d: ", i + 1);
        scanf("%d", &board[i]);
    }
}
void evaluate_proposed_combination(
    enum color_t hidden_combination[], 
    enum color_t proposed_combination[], 
    int *num_well_placed_pawns, 
    int *num_misplaced_pawns) {
    
    *num_well_placed_pawns = 0;
    *num_misplaced_pawns = 0;
    
    for (int i = 0; i < NUM_PAWNS; ++i) {
        if (proposed_combination[i] == hidden_combination[i]) {
            (*num_well_placed_pawns)++;
        }
    }

    for (int i = 0; i < NUM_PAWNS; ++i) {
        for (int j = 0; j < NUM_PAWNS; ++j) {
            if (i != j && proposed_combination[i] == hidden_combination[j]) {
                (*num_misplaced_pawns)++;
            }
        }
    }
}

void game() {
    enum color_t hidden_combination[NUM_PAWNS];
    enum color_t proposed_combination[NUM_PAWNS];
    int num_well_placed_pawns, num_misplaced_pawns;
    
    generate_hidden_combination(hidden_combination);

    for (int attempt = 1; attempt <= NUM_ATTEMPTS; ++attempt) {
        read_proposed_combination(proposed_combination);

        evaluate_proposed_combination(
            hidden_combination, 
            proposed_combination, 
            &num_well_placed_pawns, 
            &num_misplaced_pawns
        );

        printf("Attempt %d: Well-placed pawns: %d, Misplaced pawns: %d\n",
               attempt, num_well_placed_pawns, num_misplaced_pawns);

        if (num_well_placed_pawns == NUM_PAWNS) {
            printf("Congratulations! You guessed the hidden combination.\n");
            break;
        }
    }

    printf("Game over. The hidden combination was:");
    for (int i = 0; i < NUM_PAWNS; ++i) {
        printf(" %d", hidden_combination[i]);
    }
    printf("\n");
}

int main() {
    game();
    return 0;
}

