{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "name": "selisih_mundur_fix.ipynb",
      "provenance": [],
      "collapsed_sections": []
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    }
  },
  "cells": [
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "t8NNv8OFrGrb"
      },
      "source": [
        "# Final Project Mata Kuliah Metode Numerik ( Metode Selisih Mundur )\n",
        "\n",
        "Kelas : Metode Numerik D\n",
        "\n",
        "Kelompok 4 :\n",
        "1. Aan Evian Nanda (19081010040)\n",
        "2. Putu Nandhika Pratama A (19081010143)\n",
        "3. Alif Ernanda Putra (19081010132)\n",
        "4. Ahmad Dendy Prasongko P (19081010180)\n",
        "5. Muhammad Alfyando (19081010037)\n",
        "6. Saiful Abroriy (19081010045)\n",
        "\n",
        "### Algoritma Metode Selisih Mundur :\n",
        "1. Definisikan fungsi f(x) yang akan di cari nilai turunannya\n",
        "2. Masukan nilai iterasi maksimal\n",
        "3. Masukan nilai pendekatan awal : batas atas (a), batas bawah (b)\n",
        "4. Mencari nilai h (step) dengan rumus :\n",
        "   ### - $h = \\frac{(xb-xa)}{IterasiMaksimal}$\n",
        "5. Mencari nilai x - h\n",
        "6. Mensubstitusikan nilai x ke persamaan\n",
        "7. Mensubstitusikan nilai x - h ke persamaan\n",
        "8. Mencari turunan f(x) menggunakan rumus :\n",
        "   ### - $f'(x) = \\frac{f(x)-f(x-h)}{h}$\n",
        "9. Mencari turunan f(x) dengan cara eksak\n",
        "10. Mencari error dengan rumus :\n",
        "   ### - $E(f) =  \\frac{-2f(x+h)+f(x)-f(x+2h)}{2h}$  \n",
        "   atau  f'(x)-f'(x) eksak\n",
        "11. Menambahkan nilai x dengan nilai h (stap)\n",
        "12. Tampilkan Nilai i, x, x-h, f(x), f(x-h), f ‘(x), f‘(x) eksak dan galat/error\n",
        "13. Kembali Ke langkah 5 sampai nilai a sama dengan nilai b\n",
        "14. Selesai\n",
        "\n"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "6zSaex0Md-LV"
      },
      "source": [
        "### A. Import Library"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "yl2prF5frEdz"
      },
      "source": [
        "from tabulate import tabulate\n",
        "from math import *"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "WmPlGmFN07_H"
      },
      "source": [
        "from sympy.interactive import printing\n",
        "printing.init_printing(use_latex=True)\n",
        "from sympy import *\n",
        "import sympy as sp\n",
        "\n",
        "x = sp.Symbol('x')"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "Znmq7kJRsv3r"
      },
      "source": [
        "### B. Deklarasi Fungsi"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "Skl5YymoOpXs"
      },
      "source": [
        "1. Fungsi f(x, pers)"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "zv5B7XF0s0gR"
      },
      "source": [
        "def f(x, pers):\n",
        "  \"\"\"\n",
        "    Menguraikan persamaan mmenjadi ekspresi python.\n",
        "  \"\"\"\n",
        "  return (eval(pers))"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "5D8RVmYHPATk"
      },
      "source": [
        "2. Fungsi change_equation(pers)"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "k5cmpRYPPAd_"
      },
      "source": [
        "def change_equation(pers):\n",
        "  \"\"\"\n",
        "    Mengubah simbol '^' -> '**'\n",
        "                    'e' -> nilai e yaitu 2.718...\n",
        "  \"\"\"\n",
        "  if \"^\" in pers:\n",
        "    pers = pers.replace(\"^\", \"**\")\n",
        "  if \"e\" in pers:\n",
        "    pers = pers.replace(\"e\", str(exp(1)))\n",
        "  return (pers)"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "2EOyBC_-PAxJ"
      },
      "source": [
        "3. Fungsi find_step(xa, xb, n)"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "BoSZ-qsEPA73"
      },
      "source": [
        "def find_step(xa, xb, n):\n",
        "  \"\"\"\n",
        "    Mencari step dengan batas atas dikurangi batas bawah dibagi dengan iterasi maksimal\n",
        "  \"\"\"\n",
        "  return ((xb-xa)/n)"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "Ez3wB9MnTKEr"
      },
      "source": [
        "4. Fungsi do_diff_mundur(pers)"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "29ErkUNAUMte"
      },
      "source": [
        "def do_diff_mundur(fx, fxh, h):\n",
        "  \"\"\"\n",
        "    Melakukan turunan dengan cara selisih mundur\n",
        "  \"\"\"\n",
        "  return (str((fx - fxh)/h)) #alias rumus f(x)-f(x-h)/h"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "C_jYx3zhVGrm"
      },
      "source": [
        "5. Fungsi do_diff_eksak(pers)"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "hCZ3ENU5VOkF"
      },
      "source": [
        "def do_diff_eksak(pers):\n",
        "  \"\"\"\n",
        "    Melakukan turunan dengan cara eksak / biasa\n",
        "  \"\"\"\n",
        "  return (str(diff(pers, x)))"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "2U5BD-JQnOim"
      },
      "source": [
        "6. Fungsi show"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "jOmeAcGrnOvU"
      },
      "source": [
        "def show(body, header):\n",
        "  \"\"\"\n",
        "    Menampilkan output berupa tabel\n",
        "  \"\"\"\n",
        "  print(tabulate(body, headers=header, floatfmt=\".6f\"))"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "2RCg-mAzbh9f"
      },
      "source": [
        "7. Fungsi selisih_mundur()"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "n-iF1t9PbiQT"
      },
      "source": [
        "def selisih_mundur():\n",
        "  print(\"\\t\\t\\t\\t  Differensiasi Numerik Selisih Mundur\")\n",
        "  print(\"\\t\\t\\t\\t=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=\\n\")\n",
        "\n",
        "  p = input(\"Persamaan :\")\n",
        "  persamaan = change_equation(p)\n",
        "  n = int(input(\"Masukkan iterasi maksimal : \"))\n",
        "  print()\n",
        "\n",
        "  a = float(input(\"Masukkan batas bawah : \"))\n",
        "  b = float(input(\"Masukkan batas atas : \"))\n",
        "  print()\n",
        "\n",
        "  res = []\n",
        "  i = 0\n",
        "  \n",
        "  h = find_step(a, b, n)\n",
        "  x = a\n",
        "\n",
        "  total_e = 0\n",
        "\n",
        "  while(True):\n",
        "    xh = x - h\n",
        "    fx = f(x, persamaan)\n",
        "    fxh = f(xh, persamaan)\n",
        "    p_turunan_mundur = do_diff_mundur(fx, fxh, h)\n",
        "    p_turunan_eksak = do_diff_eksak(persamaan)\n",
        "    fturunan_mundur = f(x, p_turunan_mundur)\n",
        "    fturunan_eksak = f(x, p_turunan_eksak)\n",
        "    e = abs(fturunan_mundur - fturunan_eksak)\n",
        "    res.append([i, x, xh, fx, fxh, fturunan_mundur, fturunan_eksak, e])\n",
        "    total_e += e\n",
        "    if i > n or x > b:\n",
        "      show(res, [\"i\", \"x\", \"x - h\", \"f(x)\", \"f(x - h)\", \"f'(x)\", \"f'(x) eksak\", \"e\"])\n",
        "      e_avg = total_e/(i + 1)\n",
        "      print(f\"\\nrata-rata error = {e_avg}\")\n",
        "      break\n",
        "\n",
        "    i += 1\n",
        "    x += h"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "3pandQnUs73W"
      },
      "source": [
        "### C. Output"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "vIAaBNJgtKPQ",
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "e245d3bd-1c5f-4b27-f488-066f447eb8de"
      },
      "source": [
        "selisih_mundur()"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "\t\t\t\t  Differensiasi Numerik Selisih Mundur\n",
            "\t\t\t\t=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=\n",
            "\n",
            "Persamaan :e^(-x)*sin(2*x)+1\n",
            "Masukkan iterasi maksimal : 20\n",
            "\n",
            "Masukkan batas bawah : 0\n",
            "Masukkan batas atas : 1\n",
            "\n",
            "  i         x      x - h      f(x)    f(x - h)      f'(x)    f'(x) eksak         e\n",
            "---  --------  ---------  --------  ----------  ---------  -------------  --------\n",
            "  0  0.000000  -0.050000  1.000000    0.895048   2.099040       2.000000  0.099040\n",
            "  1  0.050000   0.000000  1.094964    1.000000   1.899290       1.797990  0.101300\n",
            "  2  0.100000   0.050000  1.179763    1.094964   1.695979       1.593838  0.102141\n",
            "  3  0.150000   0.100000  1.254357    1.179763   1.491863       1.390175  0.101688\n",
            "  4  0.200000   0.150000  1.318829    1.254357   1.289443       1.189373  0.100070\n",
            "  5  0.250000   0.200000  1.373377    1.318829   1.090964       0.993547  0.097417\n",
            "  6  0.300000   0.250000  1.418297    1.373377   0.898409       0.804550  0.093859\n",
            "  7  0.350000   0.300000  1.453973    1.418297   0.713502       0.623978  0.089524\n",
            "  8  0.400000   0.350000  1.480858    1.453973   0.537713       0.453175  0.084538\n",
            "  9  0.450000   0.400000  1.499471    1.480858   0.372262       0.293241  0.079022\n",
            " 10  0.500000   0.450000  1.510378    1.499471   0.218133       0.145042  0.073091\n",
            " 11  0.550000   0.500000  1.514182    1.510378   0.076079       0.009222  0.066857\n",
            " 12  0.600000   0.550000  1.511514    1.514182  -0.053360      -0.113782  0.060421\n",
            " 13  0.650000   0.600000  1.503021    1.511514  -0.169848      -0.223728  0.053880\n",
            " 14  0.700000   0.650000  1.489360    1.503021  -0.273233      -0.320553  0.047321\n",
            " 15  0.750000   0.700000  1.471183    1.489360  -0.363532      -0.404355  0.040824\n",
            " 16  0.800000   0.750000  1.449137    1.471183  -0.440918      -0.475378  0.034460\n",
            " 17  0.850000   0.800000  1.423852    1.449137  -0.505700      -0.533992  0.028292\n",
            " 18  0.900000   0.850000  1.395937    1.423852  -0.558309      -0.580684  0.022375\n",
            " 19  0.950000   0.900000  1.365973    1.395937  -0.599277      -0.616032  0.016755\n",
            " 20  1.000000   0.950000  1.334512    1.365973  -0.629225      -0.640696  0.011471\n",
            "\n",
            "rata-rata error = 0.0668735940886081\n"
          ],
          "name": "stdout"
        }
      ]
    }
  ]
}
