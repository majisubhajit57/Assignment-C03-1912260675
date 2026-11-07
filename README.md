problem1
#include <stdio.h>

int steps; /* comparisons made by the last search */

/* Linear search for unsorted arrays */
int linear_search(const int a[], int n, int key) {
    steps = 0;
    for (int i = 0; i < n; i++) {
        steps++;
        if (a[i] == key) return i;
    }
    return -1;
}

/* Binary search for sorted arrays in ascending order */
int binary_search(const int a[], int n, int key) {
    steps = 0;
    int low = 0, high = n - 1;
    
    // TODO 1 Completed
    while (low <= high) {
        int mid = low + (high - low) / 2;
        steps++;
        if (a[mid] == key) {
            return mid;
        } else if (a[mid] < key) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return -1;
}

int main(void) {
    int accession[] = {1021, 1048, 1103, 1190, 1222, 1305, 1377, 1402, 1460, 1533};
    int n = sizeof(accession) / sizeof(accession[0]);
    int keys[] = {1021, 1305, 1533, 1400};
    
    printf("--- Binary Search Results ---\n");
    for (int k = 0; k < 4; k++) {
        int pos = binary_search(accession, n, keys[k]);
        printf("Book %d -> index %d, comparisons = %d\n", keys[k], pos, steps);
    }
    return 0;
}
problem2
#include <stdio.h>
#include <stdbool.h>

void print_array(int arr[], int n) {
    for(int i = 0; i < n; i++) printf("%d ", arr[i]);
    printf("\n");
}

void bubble_sort(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        bool swapped = false; // Early-exit flag
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                swapped = true;
            }
        }
        if (!swapped) break; // Exit if already sorted
    }
}

void selection_sort(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int min_idx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[min_idx]) min_idx = j;
        }
        int temp = arr[min_idx];
        arr[min_idx] = arr[i];
        arr[i] = temp;
    }
}

void insertion_sort(int arr[], int n) {
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j = j - 1;
        }
        arr[j + 1] = key;
    }
}

int main(void) {
    int seating[] = {105, 102, 108, 101, 109, 103};
    int n = sizeof(seating) / sizeof(seating[0]);
    
    printf("Original: ");
    print_array(seating, n);
    
    insertion_sort(seating, n);
    
    printf("Sorted:   ");
    print_array(seating, n);
    return 0;
}
problem3
#include <stdio.h>

/* Copies both into out[] in descending order and returns the total count. */
int merge(const int a[], int na, const int b[], int nb, int out[]) {
    int i = 0, j = 0, k = 0;
    
    // TODO 1 Completed: Merge while both lists have elements
    while (i < na && j < nb) {
        if (a[i] >= b[j]) {
            out[k++] = a[i++];
        } else {
            out[k++] = b[j++];
        }
    }
    
    // Copy remaining elements of a[]
    while (i < na) {
        out[k++] = a[i++];
    }
    
    // Copy remaining elements of b[]
    while (j < nb) {
        out[k++] = b[j++];
    }
    
    return k;
}

int main(void) {
    int campusA[] = {98, 91, 85, 72};
    int campusB[] = {95, 91, 80};
    int merged[7];
    
    int n = merge(campusA, 4, campusB, 3, merged);
    
    printf("Expected output:\n98 95 91 91 85 80 72\n");
    printf("Actual output:\n");
    for (int i = 0; i < n; i++) {
        printf("%d ", merged[i]);
    }
    printf("\n");
    
    return 0;
}
problem4
#include <stdio.h>

#define DISTRICTS 38
#define MONTHS 12

// 1. Compute annual total per district (Row Totals)
void compute_annual_totals(double rain[DISTRICTS][MONTHS], double district_totals[DISTRICTS]) {
    for (int i = 0; i < DISTRICTS; i++) {
        district_totals[i] = 0;
        for (int j = 0; j < MONTHS; j++) {
            district_totals[i] += rain[i][j];
        }
    }
}

// 2. Compute monthly average for the state (Column Averages)
void compute_monthly_averages(double rain[DISTRICTS][MONTHS], double monthly_avgs[MONTHS]) {
    for (int j = 0; j < MONTHS; j++) {
        double sum = 0;
        for (int i = 0; i < DISTRICTS; i++) {
            sum += rain[i][j];
        }
        monthly_avgs[j] = sum / DISTRICTS;
    }
}

// 3. Find the wettest district-month (Maximum Element)
void find_maximum_rainfall(double rain[DISTRICTS][MONTHS]) {
    double max_rain = rain[0][0];
    int max_dist = 0, max_month = 0;
    
    for (int i = 0; i < DISTRICTS; i++) {
        for (int j = 0; j < MONTHS; j++) {
            if (rain[i][j] > max_rain) {
                max_rain = rain[i][j];
                max_dist = i;
                max_month = j;
            }
        }
    }
    printf("Wettest Month: District %d, Month %d (%.2f mm)\n", max_dist, max_month, max_rain);
}

// 4. List deficient districts (< 60% of state average)
void list_deficient_districts(double district_totals[DISTRICTS]) {
    double state_total = 0;
    for (int i = 0; i < DISTRICTS; i++) {
        state_total += district_totals[i];
    }
    double state_avg = state_total / DISTRICTS;
    double threshold = 0.60 * state_avg;
    
    printf("Deficient Districts (Below %.2f mm):\n", threshold);
    for (int i = 0; i < DISTRICTS; i++) {
        if (district_totals[i] < threshold) {
            printf("- District %d (%.2f mm)\n", i, district_totals[i]);
        }
    }
}

int main(void) {
    double rain[DISTRICTS][MONTHS] = {0}; // Initialize with data in real scenario
    double district_totals[DISTRICTS];
    double monthly_avgs[MONTHS];
    
    // Example computations
    compute_annual_totals(rain, district_totals);
    compute_monthly_averages(rain, monthly_avgs);
    find_maximum_rainfall(rain);
    list_deficient_districts(district_totals);
    
    return 0;
}
problem5
#include <stdio.h>
#include <ctype.h>

int word_count(const char s[]) {
    int count = 0, inside = 0;
    // TODO 1 Completed
    for (int i = 0; s[i] != '\0'; i++) {
        if (!isspace((unsigned char)s[i])) { // If it's a non-space character
            if (!inside) {
                count++;       // We found a new word
                inside = 1;    // We are now inside a word
            }
        } else {
            inside = 0;        // Space found, we are outside a word
        }
    }
    return count;
}

int is_palindrome(const char s[]) {
    int left = 0, right = 0;
    while (s[right] != '\0') right++;
    if (right > 0) right--;

    // TODO 2 Completed
    while (left < right) {
        // Skip non-letters from left
        while (left < right && !isalpha((unsigned char)s[left])) left++;
        // Skip non-letters from right
        while (left < right && !isalpha((unsigned char)s[right])) right--;

        // Compare ignoring case
        if (tolower((unsigned char)s[left]) != tolower((unsigned char)s[right])) {
            return 0; // Mismatch found
        }
        left++;
        right--;
    }
    return 1;
}

int main(void) {
    printf("Words: %d\n", word_count(" Meet at the library at 5 PM "));
    printf("%d %d\n", is_palindrome("Madam, I'm Adam"), is_palindrome("Hello"));
    return 0;
}
problem6
#include <stdio.h>
#include <string.h>

#define MAX_NAMES 500
#define MAX_LEN 30

// Sorts the names alphabetically
void sort_names(char names[][MAX_LEN], int n) {
    char temp[MAX_LEN];
    for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
            if (strcmp(names[i], names[j]) > 0) {
                strcpy(temp, names[i]);
                strcpy(names[i], names[j]);
                strcpy(names[j], temp);
            }
        }
    }
}

// Binary search to find a specific name
int search_name(char names[][MAX_LEN], int n, const char* target) {
    int low = 0, high = n - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        int cmp = strcmp(names[mid], target);
        if (cmp == 0) return mid; // Found
        if (cmp < 0) low = mid + 1;
        else high = mid - 1;
    }
    return -1; // Not found
}
problem7
#include <stdio.h>

#define MAX_CANDIDATES 12

int main() {
    // Sample vote dataset (0 and 15 are invalid)
    int votes[] = {1, 2, 1, 15, 0, -3, 3, 2, 2, 1, 1}; 
    int n_votes = sizeof(votes) / sizeof(votes[0]);

    // Arrays: 1-indexed to easily match candidate 1 through 12
    int counts[MAX_CANDIDATES + 1] = {0}; 
    int valid = 0, invalid = 0;

    // One-pass Counting Method
    for (int i = 0; i < n_votes; i++) {
        if (votes[i] >= 1 && votes[i] <= MAX_CANDIDATES) {
            counts[votes[i]]++;
            valid++;
        } else {
            invalid++;
        }
    }

    // Sorting / Tie-Detection Logic
    int max_votes = -1;
    int winner = -1;
    int tie = 0;

    for (int i = 1; i <= MAX_CANDIDATES; i++) {
        if (counts[i] > max_votes) {
            max_votes = counts[i];
            winner = i;
            tie = 0; // Reset tie if a new strict maximum is found
        } else if (counts[i] == max_votes && max_votes > 0) {
            tie = 1; // Mark tie if another candidate shares the highest count
        }
    }

    // Print Results
    printf("Total Valid Votes: %d\n", valid);
    printf("Total Invalid Votes: %d\n", invalid);

    if (tie) {
        printf("Election Result: TIE detected at %d votes.\n", max_votes);
    } else if (winner != -1) {
        printf("Election Result: WINNER is Candidate %d with %d votes.\n", winner, max_votes);
    }

    return 0;
}
