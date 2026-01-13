# 📉 Market Risk Engine: Single-Asset GARCH-FHS (CEMEX Case Study)

![Python](https://img.shields.io/badge/Python-3.9%2B-blue?style=for-the-badge&logo=python)
![Risk Model](https://img.shields.io/badge/Model-GARCH_(1,1)-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Backtest-Passed_(4.05%25)-success?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

## 📌 Resumen Ejecutivo
Este proyecto implementa un motor de **Riesgo de Mercado Cuantitativo** diseñado específicamente para activos de Mercados Emergentes (como la BMV). 

A diferencia de los modelos paramétricos tradicionales que asumen Normalidad (Curva de Gauss), este motor aborda dos hechos estilizados cruciales de los activos financieros mexicanos:
1.  **Clustering de Volatilidad:** La volatilidad no es constante, se agrupa en el tiempo.
2.  **Leptocurtosis (Fat Tails):** Los eventos extremos ocurren con mayor frecuencia de lo que predice una distribución normal.

El modelo fue calibrado con datos de **CEMEX (CEMEXCPO.MX)** obteniendo una tasa de éxito en el Backtesting del **95.95%** (Tasa de fallo del 4.05%), validando su robustez para la gestión de capital.

## 🧬 Metodología Cuantitativa

El motor sigue el framework teórico de *Statistics of Financial Markets* (Franke, Härdle & Hafner), combinando modelado econométrico con simulación estocástica.

### 1. GARCH(1,1) con Distribución t-Student
Modelamos la varianza condicional ($\sigma^2_t$) para capturar la "memoria" del mercado:

$$\sigma^2_t = \omega + \alpha \varepsilon^2_{t-1} + \beta \sigma^2_{t-1}$$

Se utilizan innovaciones con distribución **t-Student** (grados de libertad $\nu$ estimados dinámicamente) para capturar el riesgo de cola pesada.

### 2. Simulación Histórica Filtrada (FHS)
Para el pronóstico de riesgo (VaR y ES), utilizamos un enfoque híbrido:
1.  **Devolatilización:** Estandarización de residuos históricos ($z_t = r_t / \sigma_t$).
2.  **Bootstrapping:** Muestreo aleatorio de 10,000 escenarios basado en el "ruido" empírico.
3.  **Proyección:** Re-escalamiento con la volatilidad pronosticada para $T+1$.

## 📊 Resultados de Validación (Backtesting)

El modelo fue sometido a una prueba retrospectiva ("Reality Check") comparando el VaR 95% estimado contra los retornos reales diarios (2020-2025).

| Métrica | Resultado | Objetivo Teórico | Interpretación |
| :--- | :--- | :--- | :--- |
| **Total Observaciones** | ~1,250 días | - | Muestra representativa (incluye crisis COVID). |
| **Tasa de Fallo** | **4.05%** | 5.00% | **Modelo Robusto y Conservador**. |
| **Estatus** | ✅ ACEPTADO | 4% - 6% | Cumple con estándares de gestión de riesgo. |

### Visualización del Backtesting
*La línea roja representa el límite de riesgo dinámico. Los puntos magenta son las excepciones donde el mercado rompió la protección.*
![Backtesting Graph](img/backtesting_result.png)

## 🔮 Pronóstico de Riesgo (Risk Forecasting)

El motor genera métricas de riesgo monetario para la toma de decisiones diaria (Mesa de Dinero).

**Ejemplo de Output (Enero 2026):**
* **VaR 95%:** Escudo primario de riesgo.
* **Expected Shortfall (CVaR):** Pérdida promedio esperada en caso de colapso del mercado (Riesgo de Cola).

*Distribución de pérdidas simuladas para T+1:*
![Distribution Graph](img/distribution_forecast.png)

## 🛠️ Instalación y Uso

1. **Clonar el repositorio:**
   ```bash
   git clone [https://github.com/TU_USUARIO/Market-Risk-GARCH.git](https://github.com/TU_USUARIO/Market-Risk-GARCH.git)
