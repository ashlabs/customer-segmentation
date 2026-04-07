# Customer Segmentation — Feature Decisions Log

## Overview

This document captures feature-level decisions made during preprocessing and feature engineering for the customer segmentation project. Each feature is evaluated based on relevance, redundancy, and suitability for clustering.

---

## Feature Decision Table

| Field                                                                         | Group        | Action                                   | Reasoning                                                                                                               |
| ----------------------------------------------------------------------------- | ------------ | ---------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| ANREDE_KZ                                                                     | Demographics | Drop                                     | Weak segmentation signal; gender does not strongly differentiate clusters compared to behavioral and financial features |
| NATIONALITAET_KZ                                                              | Demographics | Drop                                     | Derived/inferred attribute with high uncertainty and noise                                                              |
| GEBURTSJAHR                                                                   | Demographics | Transform → AGE                          | Converted to meaningful numeric age feature; raw year not useful                                                        |
| ALTER_HH                                                                      | Household    | Transform → HH_AGE                       | Renamed to improve interpretability; already ordinal                                                                    |
| PRAEGENDE_JUGENDJAHRE                                                         | Behavior     | Transform → YOUTH_MOVEMENT, YOUTH_REGION | Encodes two separate signals (movement + region); split improves feature clarity                                        |
| WOHNLAGE                                                                      | Housing      | Transform (clean)                        | Converted to numeric and cleaned (0 → NaN); valid ordinal feature                                                       |
| MIN_GEBAEUDEJAHR                                                              | Building     | Transform → BUILDING_AGE                 | Converted to age for better interpretability                                                                            |
| CAMEO_DEU_2015                                                                | Wealth       | Transform then Drop                      | Split attempted but replaced by better structured INTL version                                                          |
| CAMEO_INTL_2015                                                               | Wealth       | Transform → WEALTH + LIFE_STAGE          | Provides compact, structured socioeconomic segmentation                                                                 |
| CAMEO_DEUG_2015                                                               | Wealth       | Drop                                     | Redundant with INTL version; less informative                                                                           |
| FINANZTYP                                                                     | Financial    | Drop                                     | Redundant with detailed FINANZ_* features                                                                               |
| FINANZ_*                                                                      | Financial    | Keep                                     | Strong behavioral/financial indicators; key segmentation drivers                                                        |
| SEMIO_REL, SEMIO_VERT, SEMIO_KAEM                                             | Personality  | Drop                                     | Low incremental value; reduces noise and redundancy                                                                     |
| SEMIO_KULT, SEMIO_DOM, SEMIO_PFLICHT, SEMIO_TRADV                             | Personality  | Drop                                     | Over-representation of personality features; reduces dimensional dominance                                              |
| SEMIO_SOZ, SEMIO_FAM, SEMIO_MAT, SEMIO_LUST, SEMIO_ERL, SEMIO_RAT, SEMIO_KRIT | Personality  | Keep                                     | Balanced subset capturing diverse personality traits                                                                    |
| HEALTH_TYP                                                                    | Demographics | Drop                                     | Weak correlation with customer segmentation                                                                             |
| INNENSTADT                                                                    | Geography    | Keep                                     | Reflects urban proximity; impacts behavior and access                                                                   |
| GEBAEUDETYP_RASTER                                                            | Geography    | Keep                                     | Indicates residential/commercial mix                                                                                    |
| MOBI_REGIO                                                                    | Geography    | Keep                                     | Strong proxy for mobility and lifestyle                                                                                 |
| REGIOTYP                                                                      | Geography    | Keep                                     | Socioeconomic regional classification                                                                                   |
| ARBEIT                                                                        | Geography    | Keep                                     | Employment level; strong economic signal                                                                                |
| KBA05_GBZ                                                                     | Housing      | Keep                                     | Building density; correlates with urbanization                                                                          |
| PLZ8_GBZ                                                                      | Housing      | Keep                                     | Regional density at different granularity                                                                               |
| PLZ8_BAUMAX                                                                   | Housing      | Keep                                     | Building structure proxy for wealth                                                                                     |
| KBA05_BAUMAX                                                                  | Housing      | Drop                                     | Redundant / less reliable mixed feature                                                                                 |
| LP_LEBENSPHASE_FEIN, LP_FAMILIE_FEIN, LP_STATUS_FEIN                          | Life Stage   | Drop                                     | Too granular; replaced with GROB versions                                                                               |
| LP_LEBENSPHASE_GROB, LP_FAMILIE_GROB, LP_STATUS_GROB                          | Life Stage   | Keep                                     | Coarse but stable life stage segmentation                                                                               |
| SOHO_KZ                                                                       | Behavior     | Keep                                     | Indicates home-office behavior; useful segmentation signal                                                              |
| VERS_TYP                                                                      | Behavior     | Keep                                     | Risk profile (safety vs risk-taking)                                                                                    |
| OST_WEST_KZ                                                                   | Geography    | Transform → Binary                       | Converted from categorical (W/O) to numeric (0/1)                                                                       |
| ANZ_PERSONEN                                                                  | Household    | Keep                                     | Household size; important demographic factor                                                                            |
| HH_EINKOMMEN_SCORE                                                            | Household    | Keep                                     | Strong proxy for income                                                                                                 |
| WOHNDAUER_2008                                                                | Household    | Keep                                     | Stability indicator                                                                                                     |
| ONLINE_AFFINITAET                                                             | Behavior     | Keep                                     | Digital engagement indicator                                                                                            |
| RETOURTYP_BK_S                                                                | Behavior     | Keep                                     | Return behavior; strong signal for commerce                                                                             |
| CJT_GESAMTTYP                                                                 | Behavior     | Keep                                     | Customer type segmentation                                                                                              |
| SHOPPER_TYP                                                                   | Behavior     | Keep                                     | Shopping behavior classification                                                                                        |
| ZABEOTYP                                                                      | Behavior     | Keep                                     | Purchasing behavior segmentation                                                                                        |
| GFK_URLAUBERTYP                                                               | Behavior     | Keep                                     | Lifestyle indicator (travel behavior)                                                                                   |
| GREEN_AVANTGARDE                                                              | Behavior     | Keep                                     | Environmental orientation; niche but useful                                                                             |
| GEBAEUDETYP                                                                   | Building     | Keep                                     | Building category; affects lifestyle context                                                                            |
| KONSUMNAEHE                                                                   | Building     | Keep                                     | Proximity to shopping facilities                                                                                        |
| ANZ_HAUSHALTE_AKTIV                                                           | Building     | Keep                                     | Activity density                                                                                                        |
| BALLRAUM, EWDICHTE, KKK, ORTSGR_KLS9, RELAT_AB                                | Geography    | Keep                                     | Regional density and classification features                                                                            |

---

## Summary

* Redundant and noisy features were removed to reduce dimensionality.
* Mixed and encoded features were transformed into interpretable components.
* Personality features were reduced to avoid over-representation.
* Socio-economic and geographic features were preserved due to strong segmentation signal.