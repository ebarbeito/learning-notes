# Hexagonal Architecture Explained

by Alistair Cockburn and Juan Manuel Garrido de Paz

------

> How the Ports & Adapters architecture simplifies your life, and how to implement it

« _Looking at the screen of my laptop, I realized that it was full of code that didn’t let me understand what it did regarding business logic. From that moment I began to search until I discovered the architecture that decouples the business logic from the frameworks: Hexagonal Architecture, more correctly called Ports & Adapters. From that moment until now, I haven’t stopped reading and learning about this pattern._ »

Related resources

* [jmgarridopaz/discounter](jmgarridopaz/discounter) — An implementation of the application included in the "Sample Code" section of "Hexagonal Architecture" pattern
* [Hexagonal architecture](https://alistair.cockburn.us/hexagonal-architecture/)
* [Hexagonal Me](https://jmgarridopaz.github.io/), by Juan Manuel Garrido de Paz
* [Hexagonal Architecture (Ports & Adapters) The 2023 version](https://alistaircockburn.com/Hexagonal%20Budapest%2023-05-18.pdf)

------

## Introduction

* This is a preview edition. 1st edition may come next 2025
* Small sample code piece (copy, paste it, give it a try), brief timeline of the pattern, associated costs and benefits

```java
// (...)/ports/driving
interface ForCalculatingTaxes {
  double taxOn(double amount);
}

// (...)/ports/driven
interface ForGettingTaxRates {
  double taxRate(double amount);
}

// (...)
class TaxCalculator implements ForCalculatingTaxes {
  public TaxCalculator(
    private ForGettingTaxRates taxRateRepository
  ) {}
	public double taxOn(double amount) {
		return amount * taxRateRepository.taxRate(amount);
	}
}

// (...)
class FixedTaxRateRepository implements ForGettingTaxRates {
	public double taxRate(double amount) {
		return 0.15;
	}
}

// (...)
class Main {
	public static void main(String[] args) {
    ForGettingTaxRates taxRateRepository = new FixedTaxRateRepository;
    TaxCalculator myCalculator = new TaxCalculator(taxRateRepository);
    System.out.printin(myCalculator.taxOn(100));
}
```

* History and name of the pattern
  * 1988 Pain #1— 1994 Pain #2 — 2000 — 2005 "Ports & Adapters" — 2015 notions of Driving / Driven adapters — 2022 special case of "Component + Strategy" / *required interfaces* concept — 2023 "Configurable Receiver" pattern replacing the "Configurable Dependency" — 2024 This book
  * Why "Hexagonal Architecture"
    * It was a placeholder name before Alistair understood what the sides of the hexagon stood for
    * Not appropriate name, since number six has no meaning

* 

### The Original Articles

* History of the pattern, evolution. Original 3 articles

## Ports & Adapters defined

* Explains the pattern. Definition of terms. The four elements of the pattern, the configurator. WHat is required and specified. Useful tricks
* 

## Code Samples

* A series of samples in several langs. From dead simple to complete samples
* Explore samples, understand how the pattern functions in different envitonments
* 

## FAQ — What and How?

* 

## FAQ — Related Concepts

* 

## Summary

* Synopsis of the book. Patern definition in short form

