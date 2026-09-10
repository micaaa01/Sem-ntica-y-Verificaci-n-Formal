### La interpretación lógica de la programación funcional en Rocq

> *"Un programa no es solo código ejecutable: puede ser, simultáneamente, un objeto matemático verificable."*

[![Rocq](https://img.shields.io/badge/Rocq-Prover-8A2BE2?style=for-the-badge)](https://rocq-prover.org/)
[![Curry–Howard](https://img.shields.io/badge/Curry–Howard-Correspondence-blue?style=for-the-badge)]()
[![CIC](https://img.shields.io/badge/CIC-C%C3%A1lculo%20de%20Construcciones%20Inductivas-orange?style=for-the-badge)]()
[![UNAM](https://img.shields.io/badge/UNAM-Facultad%20de%20Ciencias-006341?style=for-the-badge)](https://www.fciencias.unam.mx/)

---

## ¿De qué trata este trabajo?

Este artículo explora una pregunta central en la teoría de tipos y la verificación formal:

> **¿Cómo se interpreta la programación funcional en Rocq como un sistema lógico, y no sólo como un lenguaje de programación?**

La tesis central es que **Rocq integra programas, tipos, proposiciones y pruebas** dentro de un mismo marco formal —el **Cálculo de Construcciones Inductivas (CIC)**— gracias a la **correspondencia de Curry–Howard**. Bajo esta correspondencia, un mismo juicio de tipo

```
Γ ⊢ t : A
```

puede leerse de dos formas:

| Lectura | Significado |
|---|---|
| 💻 **Computacional** | `t` es un **programa** de tipo `A` |
| 🔎 **Lógica** | `t` es una **prueba** de la proposición `A` |

---

## Estructura del trabajo

```
├── I.   Introducción
├── II.  Programas, tipos y pruebas
│        ├── II-A. La implicación como función
│        ├── II-B. Proposiciones como tipos
│        ├── II-C. Demostraciones como construcción de términos
│        └── II-D. Alcance de esta lectura
├── III. Rocq y el CIC como marco lógico
│        ├── III-A. Universos y contenido lógico
│        ├── III-B. Productos dependientes y cuantificación
│        ├── III-C. Definiciones inductivas como datos y proposiciones
│        ├── III-D. Proof objects y tácticas
│        └── III-E. Cómputo dentro de la prueba
├── IV.  Caso de estudio: función, juicio semántico y prueba
├── V.   Diferencia con la programación funcional convencional
└── VI.  Conclusiones
```

---

## Conceptos clave

- **Correspondencia de Curry–Howard** — proposiciones ⇄ tipos, pruebas ⇄ términos.
- **Universos `Prop` y `Type`** — separan contenido lógico y contenido computacional dentro del mismo lenguaje.
- **Productos dependientes (`Πx : A, B(x)`)** — la cuantificación universal *es* una función.
- **Definiciones inductivas (`Inductive`)** — no solo generan datos, también generan **principios de inducción** y **proposiciones formales**.
- **Proof objects** — una demostración válida es, en el fondo, un **término bien tipado**; las tácticas solo ayudan a construirlo.

---

## Caso de estudio: `aeval` vs. `aevalR`

El corazón experimental del trabajo compara dos formas de dar semántica a expresiones aritméticas:

```coq
(* Semántica como función *)
Fixpoint aeval (a : aexp) : nat :=
  match a with
  | ANum n => n
  | APlus a1 a2 => aeval a1 + aeval a2
  end.

(* Semántica como juicio inductivo *)
Inductive aevalR : aexp -> nat -> Prop :=
  | E_ANum  : forall n, aevalR (ANum n) n
  | E_APlus : forall a1 a2 n1 n2,
      aevalR a1 n1 -> aevalR a2 n2 ->
      aevalR (APlus a1 a2) (n1 + n2).
```

Y se demuestra formalmente que **ambas coinciden**:

```coq
Theorem aeval_iff_aevalR :
  forall a n, aevalR a n <-> aeval a = n.
```

El programa que **calcula** y la relación que **describe** su comportamiento quedan unificados por una prueba verificada por el propio núcleo de Rocq.


---

## 🏁 Conclusión

> Programar en Rocq **no se reduce** a escribir funciones que calculan: es construir **objetos formales** cuyo significado computacional *y* lógico puede ser verificado dentro del propio sistema.

---

## Autoría

| Autor/a | Correo |
|---|---|
| Gael Emiliano Arreguín Salgado | emiliano.arreguin@ciencias.unam.mx |
| Mariana López Pérez | marlop@ciencias.unam.mx |
| Michelle Alanis Navarro Fierro | michellenavf0@ciencias.unam.mx |

 Facultad de Ciencias, UNAM — Ciudad de México, México

---

## Referencias principales

- Howard, W. A. — *The Formulae-as-Types Notion of Construction* (1980)
- Coquand, T. & Huet, G. — *The Calculus of Constructions* (1988)
- Pierce, B. C. et al. — *Software Foundations* (Vol. 1 & 2)
- The Rocq Development Team — *The Rocq Prover Reference Manual*
- Bertot, Y. & Castéran, P. — *Interactive Theorem Proving and Program Development*

*(Lista completa de 12 referencias disponible en el artículo original.)*
