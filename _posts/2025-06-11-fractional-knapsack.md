---
title: Fractional Knapsack
description: Fractional Knapsack mengoptimalkan nilai barang dalam tas dengan boleh memotong item, menggunakan strategi greedy berdasarkan nilai per berat tertinggi. 
date: 2025-06-11 01:00:00 +0800
categories: [Blogging]
tags: [Algorithm Problems]
---

# 📦 **Fractional Knapsack: Mengoptimalkan Nilai dengan Kapasitas Terbatas**

## Apa itu **Knapsack Problem**?

Knapsack Problem adalah sebuah masalah optimasi yang memerlukan kita untuk memilih barang-barang dengan nilai tertinggi dan berat terbatas. Terdapat dua jenis utama: **0/1 Knapsack** dan **Fractional Knapsack**. Perbedaannya? Pada **0/1 Knapsack**, kita hanya bisa memilih barang utuh atau tidak sama sekali, sedangkan pada **Fractional Knapsack**, kita bisa mengambil sebagian barang sesuai kebutuhan. 🌟

## **Fractional Knapsack**: Memahami Prinsip Dasarnya 🔍

**Fractional Knapsack** adalah masalah optimasi di mana kita bertujuan untuk memaksimalkan nilai barang yang dimasukkan ke dalam tas (knapsack) dengan kapasitas terbatas. Keunggulannya adalah kita diperbolehkan mengambil **sebagian barang** (misalnya 0.5 bagian), tidak hanya utuh. Hal ini memudahkan kita untuk mendapatkan nilai maksimal dari kapasitas yang ada. 🧳

## **Karakteristik Utama** 📊

1. **Barang memiliki berat dan nilai**: Setiap barang yang dipilih memiliki berat dan nilai yang bisa dihitung
2. **Kapasitas tas terbatas**: Tas yang digunakan memiliki kapasitas terbatas, sehingga kita harus memilih dengan bijak
3. **Barang bisa dipotong**: Anda bisa mengambil sebagian barang untuk mendapatkan nilai optimal
4. **Diselesaikan dengan algoritma greedy**: Pendekatan greedy memungkinkan kita memilih barang berdasarkan rasio nilai terhadap berat (value/weight)

## **Strategi Penyelesaian** 🔧

Untuk menyelesaikan masalah **Fractional Knapsack**, kita menggunakan **Greedy Algorithm**. Berikut langkah-langkahnya:

1. Hitung rasio **nilai/berat** untuk setiap barang
2. Urutkan barang berdasarkan rasio tersebut, mulai dari yang tertinggi
3. Masukkan barang ke dalam tas, ambil sebanyak mungkin dari barang dengan rasio tertinggi
4. Jika kapasitas tas tidak cukup untuk barang berikutnya, ambil sebagian dari barang tersebut

## **Kompleksitas Waktu** ⏱️

Waktu yang diperlukan untuk menyelesaikan masalah ini adalah **O(n log n)** untuk mengurutkan barang, dan **O(n)** untuk memasukkan barang ke dalam tas. Jadi, meskipun cukup efisien, kompleksitasnya tetap perlu diperhatikan.

## **Aplikasi Fractional Knapsack** 🌐

Di dunia nyata, **Fractional Knapsack** dapat diterapkan dalam berbagai bidang seperti:

- **Penjadwalan tugas** dengan sumber daya terbatas ⏳
- **Investasi dana** ke beberapa proyek 💰
- **Pengalokasian bandwidth** di jaringan 📡
- **Optimisasi logistik** 🚚

## **Algoritma Greedy**: Pilihan yang Tepat Saat Itu 🏆

**Greedy Algorithm** adalah pendekatan yang membuat keputusan terbaik berdasarkan kondisi saat ini. Di setiap langkah, algoritma ini memilih opsi yang paling menguntungkan, berharap keputusan tersebut akan mengarah pada solusi optimal secara keseluruhan.

## **Kesimpulan** 📌

**Fractional Knapsack** adalah masalah yang dapat diselesaikan dengan efisien menggunakan **Greedy Algorithm**. Dengan pendekatan ini, kita bisa memaksimalkan nilai barang yang dimasukkan ke dalam tas meskipun kapasitasnya terbatas. Algoritma ini sangat efektif, terutama ketika pilihan optimal lokal mengarah pada solusi terbaik secara global. 🌟