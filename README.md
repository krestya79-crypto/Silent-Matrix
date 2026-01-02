#include <iostream>
#include <limits>
#include <vector>

using namespace std;

const int KEY[2][2] = { {3, 5}, {1, 2} };
const int INV_KEY[2][2] = { {2, 22}, {26, 3} };
const int ALPHABET_SIZE = 27;

// هيكل بيانات لحفظ السجل
struct History {
    int data[1000];
    int size;
};

vector<History> historyLog;

int charToNum(char c);
char numToChar(int n);
void encrypt();
void decryptManual();
void decryptFromHistory();

int main() {
    int choice;
    while (true) {
        cout << "\n--- Hill Cipher System ---" << endl;
        cout << "1. Encrypt & Save to History" << endl;
        cout << "2. Decrypt (Manual Entry)" << endl;
        cout << "3. Decrypt from History (Saved Records)" << endl;
        cout << "4. Exit" << endl;
        cout << "Choice: ";
        if (!(cin >> choice)) break;

        cin.ignore(numeric_limits<streamsize>::max(), '\n');

        if (choice == 1) encrypt();
        else if (choice == 2) decryptManual();
        else if (choice == 3) decryptFromHistory();
        else if (choice == 4) break;
    }
    return 0;
}

void encrypt() {
    char t[1000], f[1000];
    int fi = 0, L = 0;
    History newRecord;
    newRecord.size = 0;

    cout << "Enter sentence: ";
    cin.getline(t, 1000);

    while (t[L] != '\0') {
        if (charToNum(t[L]) != -1) {
            if (t[L] >= 'a' && t[L] <= 'z') f[fi] = (char)(t[L] - 'a' + 'A');
            else f[fi] = t[L];
            fi++;
        }
        L++;
    }
    f[fi] = '\0';
    L = fi;

    if (L % 2 != 0) { f[L] = ' '; L++; f[L] = '\0'; }

    int b[2], r[2];
    cout << "Encrypted Numbers: ";
    for (int i = 0; i < L; i += 2) {
        for (int j = 0; j < 2; j++) b[j] = charToNum(f[i + j]);
        for (int r_idx = 0; r_idx < 2; r_idx++) {
            r[r_idx] = 0;
            for (int c = 0; c < 2; c++) r[r_idx] += KEY[r_idx][c] * b[c];
            r[r_idx] %= ALPHABET_SIZE;

            newRecord.data[newRecord.size++] = r[r_idx];
            cout << r[r_idx] << " ";
        }
    }
    historyLog.push_back(newRecord);
    cout << "\n[Saved to History as Record #" << historyLog.size() - 1 << "]" << endl;
}

void decryptManual() {
    int e[1000], count = 0;
    cout << "Enter encrypted numbers (End with Ctrl+Z/D): \n";
    while (cin >> e[count] && count < 1000) count++;
    cin.clear();
    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    cout << "Decrypted Text: ";
    for (int i = 0; i < count; i += 2) {
        int cb[2];
        for (int j = 0; j < 2; j++) cb[j] = e[i + j];
        for (int r_idx = 0; r_idx < 2; r_idx++) {
            int val = 0;
            for (int c = 0; c < 2; c++) val += INV_KEY[r_idx][c] * cb[c];
            val %= ALPHABET_SIZE;
            if (val < 0) val += ALPHABET_SIZE;
            cout << numToChar(val);
        }
    }
    cout << endl;
}

void decryptFromHistory() {
    if (historyLog.empty()) {
        cout << "History is empty!" << endl;
        return;
    }

    cout << "Available Records:" << endl;
    for (size_t i = 0; i < historyLog.size(); i++) {
        cout << "Record #" << i << ": ";
        for (int j = 0; j < historyLog[i].size; j++) cout << historyLog[i].data[j] << " ";
        cout << endl;
    }

    int recIdx;
    cout << "Enter Record Number to decrypt: ";
    cin >> recIdx;

    if (recIdx >= 0 && recIdx < historyLog.size()) {
        cout << "Decrypted Text: ";
        for (int i = 0; i < historyLog[recIdx].size; i += 2) {
            int cb[2];
            for (int j = 0; j < 2; j++) cb[j] = historyLog[recIdx].data[i + j];
            for (int r_idx = 0; r_idx < 2; r_idx++) {
                int val = 0;
                for (int c = 0; c < 2; c++) val += INV_KEY[r_idx][c] * cb[c];
                val %= ALPHABET_SIZE;
                if (val < 0) val += ALPHABET_SIZE;
                cout << numToChar(val);
            }
        }
        cout << endl;
    }
    else {
        cout << "Invalid record number!" << endl;
    }
}

int charToNum(char c) {
    if (c >= 'A' && c <= 'Z') return c - 'A';
    if (c >= 'a' && c <= 'z') return c - 'a';
    if (c == ' ') return 26;
    return -1;
}

char numToChar(int n) {
    if (n == 26) return ' ';
    return (char)(n + 'A');
}
