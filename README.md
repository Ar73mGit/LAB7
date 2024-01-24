#include <iostream>
#include <fstream>
using namespace std;

struct Point {
    unsigned char b, g, r;
};

int main() {
    char Header[54];
    int Width, Height;
    ifstream f1("1.bmp", ios::binary);
    f1.read(Header, 54);
    f1.seekg(18);
    f1.read((char*)&Width, 4);
    f1.seekg(22);
    f1.read((char*)&Height, 4);
    cout << "BitMap width = " << Width << endl;
    cout << "BitMap height = " << Height << endl;
    cout << "wait please . . ." << endl;
    f1.seekg(0, ios::end);
    int N = f1.tellg();
    N = N - 54;
    Point* p = new Point[N / 3];
    f1.seekg(54);
    f1.read((char*)p, N);
    f1.close();

    int w, h;
    int c = 0;
    for (h = 0; h < Height; h++)
        for (w = 0; w < Width; w++) {
            p[c].r = 255 - p[c].r;
            p[c].g = 255 - p[c].g;
            p[c].b = 255 - p[c].b;
            c++;
        }

    ofstream f2("2.bmp", ios::binary);
    f2.write(Header, 54);
    f2.write((char*)p, N);
    f2.close();


    c = 0;
    for (h = 0; h < Height; h++)
        for (w = 0; w < Width; w++) {
            p[c].r = p[c].r / 2;
            p[c].g = p[c].g;
            p[c].b = p[c].b / 2;
            c++;
        }

    ofstream f3("3.bmp", ios::binary);
    f3.write(Header, 54);
    f3.write((char*)p, N);
    f3.close();

    c = 0;
    for (h = 0; h < Height; h++)
        for (w = 0; w < Width; w++) {
            p[c].r = 255 + p[c].r * rand() % 58 - 21;
            p[c].g = 255 + p[c].g * rand() % 53 - 21;
            p[c].b = 255 + p[c].b * rand() % 52 - 21;
            c++;
        }
    ofstream f4("4.bmp", ios::binary);
    f4.write(Header, 54);
    f4.write((char*)p, N);
    f4.close();

    delete[] p;
    cout << "All done :) !!!" << endl;
    return 0;
}
