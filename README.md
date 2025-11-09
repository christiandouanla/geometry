## PROJET DE GEOMETRIE
nom et prenom: DOUANLA NGUEYO CHRISTIAN FRANCK
matricule: 25P902

## DESCRIPTION
ceci est Un  projet C++ pour des opérations géométriques 2D (vecteurs et points). Ce dépôt contient des fonctions simples pour addition, soustraction, homothétie, translation,et rotation (vecteurs et points autour d'un centre).

## Structure du projet

 src/
    geometry/
        point.h
        utils.h
        vector.h
        point.cpp
        vector.cpp
    main.cpp

## FONCTIONS PRINCIPALES
  (liées aux points)
- MakeP2f(x, y)
- Translate(point, dx dy)
- Scale(point, sx, sy)
- Scale(point, vector)
- Rotate(point, angle)
- ToString(point)

  (liées aux vecteurs)
- MakeV2f(x, y)
- MakeV2f(point1, point2)
- Add(a ,b)
- Dot(a, b)
- Normalize(v)
- Lerp(a, b, t)
- Determinant(vecteur1, vecteur2)


  ##FICHIERS DU PROJET

 geometry/point.h :
 
 #ifndef POINT_H
#define POINT_H
struct Vector2f ;

#include <string>
#include "vector.h"

struct Point2f {
    float x;
    float y;
};
Point2f MakeP2f(float x, float y);// pour la creation du point a partir de ses coordonnees
Point2f Translate(const Point2f& p, float dx, float dy); // pour la translation du point par rapport aux deplacements dx et dy 
Point2f Translate(const Point2f& p, const Vector2f& v); // pour la translation du point par rapport au vecteur v
Point2f Scale(const Point2f& p, float sx, float sy);  // pour l'homothetie du point par rapport aux facteurs sx et sy
Point2f Scale(const Point2f& p, const Vector2f& s); // pour l'homothetie du point par rapport au vecteur s
Point2f Rotate(const Point2f& p, float angleDegree);  // pour la rotation du point d'un angle en degre
std::string ToString(const Point2f& p); // pour afficher le point
#endif

geometry/vector.h :

#ifndef VECTOR_H
#define VECTOR_H
struct Point2f ;

#include "point.h"
#include <string>
#include <cmath>

struct Vector2f {
    float x;
    float y;
};

Vector2f MakeV2f(float x, float y);// pour la creation du vecteur a partir de ses coordonnees
Vector2f MakeV2f(const Point2f& a, const Point2f& b);// pour la creation du vecteur a partir de deux points
Vector2f Add(const Vector2f& a, const Vector2f& b);// pour l'addition de deux vecteurs
Vector2f Sub(const Vector2f& a, const Vector2f& b);// pour la soustraction de deux vecteurs
Vector2f Scale(const Vector2f& v, float scalar);// pour le redimensionnement du vecteur
float Dot(const Vector2f& a, const Vector2f& b);// pour le produit scalaire de deux vecteurs
float Length(const Vector2f& v);
Vector2f Normalize(const Vector2f& v);// pour la normalisation du vecteur
Vector2f Lerp(const Vector2f& a, const Vector2f& b, float t);
float Determinant(const Vector2f& a, const Vector2f& b);// pour le determinant de deux vecteurs

std::string ToString(const Vector2f& v); //pour afficher le vecteur
#endif

geometry/utils.h :

#ifndef UTILS_H
#define UTILS_H

#include <iostream>
#include <sstream>
#include <string>
#include <vector>
#include <map>

// Templates ToString et Print
template<typename T>
std::string ToString(const T& value) {
    std::ostringstream oss;
    oss << value;
    return oss.str();
}

template<typename T>
std::string ToString(const std::vector<T>& v) {
    std::ostringstream oss;
    oss << "[";
    for (size_t i = 0; i < v.size(); ++i) {
        oss << ToString(v[i]);
        if (i + 1 < v.size()) oss << ", ";
    }
    oss << "]";
    return oss.str();
}

template<typename K, typename V>
std::string ToString(const std::map<K, V>& m) {
    std::ostringstream oss;
    oss << "{";
    for (auto it = m.begin(); it != m.end(); ++it) {
        oss << ToString(it->first) << ": " << ToString(it->second);
        if (std::next(it) != m.end()) oss << ", ";
    }
    oss << "}";
    return oss.str();
}

template<typename T, typename... Args>
void Print(const T& first, const Args&... args) {
    std::cout << ToString(first);
    ((std::cout << ", " << ToString(args)), ...);
    std::cout << std::endl;
}

#endif

geometry/vector.cpp :

#include "vector.h"// pour les differentes operations sur les vecteurs
#include "utils.h" 
#include "point.h" // pour les differentes operations sur les points  
#include <cmath>// pour les fonctions mathematiques
#include <sstream>// pour la conversion en string

Vector2f MakeV2f(float x, float y) {
    return {x, y};
}

Vector2f MakeV2f(const Point2f& a, const Point2f& b) {
    return { b.x - a.x, b.y - a.y };
}

Vector2f Add(const Vector2f& a, const Vector2f& b) {
    return { a.x + b.x, a.y + b.y };
}

Vector2f Sub(const Vector2f& a, const Vector2f& b) {
    return { a.x - b.x, a.y - b.y };
}

Vector2f Scale(const Vector2f& v, float scalar) {
    return { v.x * scalar, v.y * scalar };
}

float Dot(const Vector2f& a, const Vector2f& b) {
    return a.x*b.x + a.y*b.y;
}

float Length(const Vector2f& v) {
    return std::sqrt(v.x*v.x + v.y*v.y);
}

Vector2f Normalize(const Vector2f& v) {
    float len = Length(v);
    if (len == 0.0f) return {0.0f, 0.0f};
    return { v.x / len, v.y / len };
}

float Determinant(const Vector2f& a, const Vector2f& b) {
    return a.x * b.y - a.y * b.x;
}

std::string ToString(const Vector2f& v) {
    std::ostringstream oss;
    oss << "(" << v.x << ", " << v.y << ")";
    return oss.str();
}

geometry/point.cpp :

#include "point.h"
#include "utils.h"


Point2f MakeP2f(float x, float y) { //
    return {x, y};
}

Point2f Translate(const Point2f& p, float dx, float dy) {
    return { p.x + dx, p.y + dy };
}

Point2f Translate(const Point2f& p, const Vector2f& v) {
    return { p.x + v.x, p.y + v.y };
}

Point2f Scale(const Point2f& p, float sx, float sy) {
    return { p.x * sx, p.y * sy };
}

Point2f Scale(const Point2f& p, const Vector2f& s) {
    return { p.x * s.x, p.y * s.y };
}

Point2f Rotate(const Point2f& p, float angleDegree) {
    float rad = angleDegree * 3.14159265358979323846f / 180.0f;
    float c = std::cos(rad), s = std::sin(rad);
    float x = p.x * c - p.y * s;
    float y = p.x * s + p.y * c;
    return {x, y};
}

std::string ToString(const Point2f& p) {
    std::ostringstream oss;
    oss << "(" << p.x << ", " << p.y << ")";
    return oss.str();
}

src/main.cpp :

#include "geometry/point.h"
#include "geometry/vector.h"
#include "geometry/utils.h"

int main() {
    float k, a;
    Print("       DONNEES:");
    Point2f pA = MakeP2f(2.f, 4.f);
    Print("le point A est de coordonnees:", ToString(pA));

    Point2f pB = MakeP2f(3.f, 6.f);
    Print("le point B est de coordonnees:", ToString(pB));

    Vector2f v = MakeV2f(1.f, -1.f);
    Print("le vecteur V a pour coordonnees:", ToString(v));

     Vector2f w = MakeV2f(2.f, -3.f);
    Print("le vecteur W a pour coordonnees:", ToString(w));

    k = 5;
    Print("le rapport est k=:", ToString(k) );
    a = 90;
    Print("l'angle de la rotaion est a=:", ToString(a), "degre\n" );

   

    Print("       OPERATIONS:");

    Point2f r = Translate(pA, v);
    Print("1.la translation du point A par rapport au vecteur v est :", ToString(r));

    Point2f h = Scale(pA, k, k);
    Print("2.l'homothetie du point A par le rapport k est :", ToString(h));

    Point2f R = Rotate(pA, a);
    Print("3.les nouveaux coordonnees du point A apres la rotation d'angle a sont:", ToString(R));

    Vector2f s = Sub(v, w);
    Print("4.le resultat de la soustraction des vecteur V et W est:", ToString(s));

    Vector2f ad = Add(v, w);
    Print("5.le resultat de l'addition des vecteurs V et W est:", ToString(ad));

    float ps = Dot(v, w);
    Print("6.le produit scalaire de V et W est:", ToString(ps));

    Print("7.la norme du vecteur V est:", Length(v));

    float det = Determinant(v, w);
    Print("8.le determinant des vecteurs V et W est:", det);

    Vector2f AB = MakeV2f(pA, pB);
    Print("9.le vecteur creer a partir des points A et B est le vecteur AB:", ToString(AB));

     Vector2f n = Normalize(v);
     Print("la normalisation du vecteur V est :", ToString(n));

    return 0;
}



## CE QU'ON A APPRIS :
 grace a ce projet nous avons appris l'organisation de code









