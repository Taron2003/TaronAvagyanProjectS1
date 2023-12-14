#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Structure to store data about video cards
struct VideoCard {
    char model[50];
    double power;       // Power of the video card in watts
    int memory;         // Memory capacity of the video card in gigabytes
    double base_price;  // Base price of the video card
    int quantity;       // Quantity of video cards of this type
    struct VideoCard *next;  // Pointer to the next video card in the linked list
};

// Function to calculate the price of a video card considering power and memory
double calculateCardPrice(struct VideoCard card) {
    // Coefficients to determine how price depends on power and memory
    const double power_coefficient = 0.1;
    const double memory_coefficient = 5.0;

    // Calculate price considering power and memory dependence
    double price = card.base_price + (card.power * power_coefficient) + (card.memory * memory_coefficient);
    return price;
}

// Function to create a new video card
struct VideoCard* createVideoCard() {
    struct VideoCard* new_card = (struct VideoCard*)malloc(sizeof(struct VideoCard));
    if (new_card == NULL) {
        perror("Failed to allocate memory for a new video card");
        exit(EXIT_FAILURE);
    }
    new_card->next = NULL;
    return new_card;
}

int main() {
    int num_card_types; // Number of types of video cards
    printf("Enter the number of types of video cards: ");
    scanf("%d", &num_card_types);

    // Clear the buffer after inputting numbers
    while (getchar() != '\n');

    // Linked list to store data about video cards
    struct VideoCard *first_card = NULL;
    struct VideoCard *current_card = NULL;

    // Input data for each type of video card
    for (int i = 0; i < num_card_types; ++i) {
        // Create a new video card
        struct VideoCard *new_card = createVideoCard();

        printf("\nEnter data for video card %d:\n", i + 1);

        printf("Model: ");
        fgets(new_card->model, sizeof(new_card->model), stdin);
        if (new_card->model[strlen(new_card->model) - 1] == '\n') {
            new_card->model[strlen(new_card->model) - 1] = '\0';
        }

        printf("Power (in watts): ");
        scanf("%lf", &new_card->power);

        printf("Memory capacity (in gigabytes): ");
        scanf("%d", &new_card->memory);

        printf("Base price: ");
        scanf("%lf", &new_card->base_price);

        printf("Quantity of video cards of this type: ");
        scanf("%d", &new_card->quantity);

        // Clear the buffer after inputting numbers
        while (getchar() != '\n');

        // Link the new video card to the linked list
        if (current_card == NULL) {
            // First video card in the linked list
            first_card = new_card;
            current_card = new_card;
        } else {
            // Link the new video card to the current one
            current_card->next = new_card;
            current_card = new_card;
        }
    }

    // Input production costs
    double production_costs;  // Costs to produce one video card
    printf("\nEnter production costs for one video card: ");
    scanf("%lf", &production_costs);

    // Calculate price, revenue, and profit for each type of video card
    double total_profit = 0.0;
    struct VideoCard *current = first_card;
    while (current != NULL) {
        double card_price = calculateCardPrice(*current);
        double total_price = card_price * current->quantity;
        double revenue = total_price;
        double profit = revenue - (production_costs * current->quantity);

        // Output results for each type of video card
        printf("\n+---------------------------------------+\n");
        printf("|          Data for video card           |\n");
        printf("+---------------------------------------+\n");
        printf("Model: %s\n", current->model);
        printf("Power: %.2lf watts\n", current->power);
        printf("Memory: %d GB\n", current->memory);
        printf("Base price: %.2lf\n", current->base_price);
        printf("Quantity of video cards: %d\n", current->quantity);

        printf("\nPrice of video card considering power and memory: %.2lf\n", card_price);
        printf("Total cost of video cards of this type: %.2lf\n", total_price);
        printf("Revenue from selling video cards of this type: %.2lf\n", revenue);
        printf("Profit from selling video cards of this type: %.2lf\n", profit);

        total_profit += profit;
        current = current->next;
    }

    // Output total profit
    printf("\nTotal profit from selling video cards: %.2lf\n", total_profit);

    // Free allocated memory
    current = first_card;
    while (current != NULL) {
        struct VideoCard *next = current->next;
        free(current);
        current = next;
    }

    return 0;
}

   

