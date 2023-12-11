# TaronAvagyanProjectS1
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

int main() {
    int num_card_types; // Number of types of video cards
    printf("Enter the number of types of video cards: ");
    scanf("%d", &num_card_types);

    // Clear the buffer after inputting numbers
    while (getchar() != '\n');

    // Array to store data about video cards
    struct VideoCard *video_cards;
    video_cards = (struct VideoCard *)malloc(num_card_types * sizeof(struct VideoCard));

    // Input data for each type of video card
    for (int i = 0; i < num_card_types; ++i) {
        printf("\nEnter data for video card %d:\n", i + 1);

        printf("Model: ");
        fgets(video_cards[i].model, sizeof(video_cards[i].model), stdin);
        if (video_cards[i].model[strlen(video_cards[i].model) - 1] == '\n') {
            video_cards[i].model[strlen(video_cards[i].model) - 1] = '\0';
        }

        printf("Power (in watts): ");
        scanf("%lf", &video_cards[i].power);

        printf("Memory capacity (in gigabytes): ");
        scanf("%d", &video_cards[i].memory);

        printf("Base price: ");
        scanf("%lf", &video_cards[i].base_price);

        printf("Quantity of video cards of this type: ");
        scanf("%d", &video_cards[i].quantity);

        // Clear the buffer after inputting numbers
        while (getchar() != '\n');
    }

    // Input production costs
    double production_costs;  // Costs to produce one video card
    printf("\nEnter production costs for one video card: ");
    scanf("%lf", &production_costs);

    // Calculate price, revenue, and profit for each type of video card
    double total_profit = 0.0;
    for (int i = 0; i < num_card_types; ++i) {
        double card_price = calculateCardPrice(video_cards[i]);
        double total_price = card_price * video_cards[i].quantity;
        double revenue = total_price;
        double profit = revenue - (production_costs * video_cards[i].quantity);

        // Output results for each type of video card
        printf("\n+---------------------------------------+\n");
        printf("|          Data for video card %d          |\n", i + 1);
        printf("+---------------------------------------+\n");
        printf("Model: %s\n", video_cards[i].model);
        printf("Power: %.2lf watts\n", video_cards[i].power);
        printf("Memory: %d GB\n", video_cards[i].memory);
        printf("Base price: %.2lf\n", video_cards[i].base_price);
        printf("Quantity of video cards: %d\n", video_cards[i].quantity);

        printf("\nPrice of video card considering power and memory: %.2lf\n", card_price);
        printf("Total cost of video cards of this type: %.2lf\n", total_price);
        printf("Revenue from selling video cards of this type: %.2lf\n", revenue);
        printf("Profit from selling video cards of this type: %.2lf\n", profit);

        total_profit += profit;
    }

    // Output total profit
    printf("\nTotal profit from selling video cards: %.2lf\n", total_profit);

    // Free allocated memory
    free(video_cards);

    return 0;
}





