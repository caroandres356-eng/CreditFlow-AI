# CreditFlow-AI

Sistema inteligente de autenticación y aprobación de créditos que utiliza IA para evaluar historiales crediticios y predecir riesgos.

---

## 1. Descripción del Proyecto
**Sistema de detección de fraude en transacciones financieras en tiempo real**

Este proyecto consiste en el desarrollo de una plataforma capaz de simular, procesar y analizar transacciones financieras para detectar comportamientos fraudulentos utilizando técnicas de machine learning.

El sistema simula un entorno financiero donde miles de transacciones ocurren simultáneamente. Estas transacciones son procesadas por un backend concurrente desarrollado en Go, el cual se encarga de recibir, distribuir y gestionar las transacciones generadas.

Posteriormente, un servicio de análisis implementado en Python utiliza algoritmos de aprendizaje automático para evaluar cada transacción y determinar la probabilidad de fraude.

Los resultados son visualizados en tiempo real mediante una interfaz desarrollada con React, que permite monitorear la actividad del sistema, visualizar patrones de fraude y analizar el comportamiento de las transacciones.

El objetivo del sistema es replicar, a escala simulada, el funcionamiento de los sistemas antifraude utilizados por instituciones financieras modernas.

## 2. Objetivos del Proyecto

**Objetivo general**
Desarrollar un sistema distribuido capaz de simular y analizar transacciones financieras en tiempo real para detectar posibles fraudes utilizando técnicas de machine learning.

**Objetivos específicos**
- Simular un flujo masivo de transacciones financieras concurrentes.
- Procesar eventos de transacciones mediante un backend concurrente.
- Analizar patrones de comportamiento mediante modelos de machine learning.
- Visualizar métricas y alertas de fraude en un dashboard interactivo.
- Diseñar una arquitectura escalable basada en microservicios.

## 3. Arquitectura del Sistema

La arquitectura del sistema se basa en una separación clara de responsabilidades entre los distintos componentes.

```text
Frontend (React Dashboard)
          │
          │ HTTP API
          ▼
Backend API (Go)
Transaction Processing
          │
          │ Event Stream
          ▼
Message Broker
(Kafka / RabbitMQ)
          │
          ▼
Machine Learning Service
(Python)
          │
          ▼
Database + Analytics
```

## 4. Tecnologías Utilizadas

### Backend
- **Lenguaje:** Go
- **Razones:** Excelente manejo de concurrencia mediante goroutines, bajo consumo de recursos, ideal para sistemas de alto throughput, muy usado en sistemas financieros y de infraestructura.
- **Framework sugerido:** Gin (web framework)
- **Se encargará de:** Recibir transacciones, generar simulaciones, exponer APIs REST, enviar transacciones al sistema de análisis.

### Frontend
- **Framework:** React
- **Razones:** Desarrollo de interfaces dinámicas, manejo eficiente de estados, amplio ecosistema de librerías de visualización.
- **Librerías útiles:** Recharts, Chart.js
- **El frontend mostrará:** Número de transacciones por segundo, porcentaje de fraude detectado, alertas en tiempo real, estadísticas de comportamiento.

### Machine Learning
- **Lenguaje:** Python
- **Bibliotecas principales:** scikit-learn, pandas, NumPy
- **Razones:** Ecosistema fuerte en análisis de datos, librerías optimizadas para machine learning, facilidad para experimentar con modelos.

### Mensajería (Opcional pero recomendado)
- **Tecnologías:** Apache Kafka o RabbitMQ
- **Razones:** Permite desacoplar los servicios y manejar streams de eventos de transacciones.

### Base de Datos
- **Base principal:** PostgreSQL
- **Razones:** Robustez, soporte para consultas analíticas, muy usado en fintech.
- **Opcional para cache o métricas:** Redis

## 5. Simulación de Transacciones

Para poder probar el sistema sin datos reales se desarrollará un simulador de transacciones financieras. El simulador generará eventos que representan operaciones bancarias como pagos, transferencias y compras con tarjeta.

Cada transacción contendrá atributos como:
- `transaction_id`
- `user_id`
- `amount`
- `timestamp`
- `merchant`
- `location`
- `device_type`

**Generación de carga**
El simulador usará goroutines de Go para generar miles de transacciones concurrentes.

*Ejemplo conceptual:* 100 workers, donde cada worker genera 100 transacciones por segundo.
*Resultado:* 10,000 transacciones por segundo, permitiendo evaluar cómo el sistema maneja alta concurrencia.

## 6. Proceso de Análisis de Fraude

El servicio de machine learning analiza cada transacción para determinar si existe una probabilidad de fraude. El modelo evalúa características como:
- Monto de la transacción
- Frecuencia de transacciones
- Ubicación geográfica
- Patrón histórico del usuario

*Ejemplo de salida:*
```json
{
  "Transaction ID": "548291",
  "Fraud Probability": 0.87,
  "Risk Level": "HIGH"
}
```

Los algoritmos recomendados para este tipo de problema incluyen:
- Random Forest
- Gradient Boosting
- Isolation Forest (detección de anomalías)

## 7. Flujo completo del sistema

1. **Paso 1:** Un usuario o simulador genera una transacción desde el sistema. El frontend o simulador hace un `POST /transaction`.
2. **Paso 2:** El backend en Go recibe la transacción. Sus funciones son validar datos, asignar ID, almacenar temporalmente y enviar evento al sistema de análisis.
3. **Paso 3:** La transacción es enviada a un broker de eventos, permitiendo procesar eventos asincrónicamente.
4. **Paso 4:** El servicio de machine learning en Python consume la transacción. Proceso:
   1. Analizar features
   2. Ejecutar modelo
   3. Calcular score de fraude
5. **Paso 5:** El resultado se almacena en la base de datos (Ej: `transaction_id`, `fraud_score`, `status`, `timestamp`).
6. **Paso 6:** El frontend consulta la API para visualizar resultados mediante `GET /fraud-alerts`. El dashboard actualiza alertas, estadísticas y métricas en tiempo real.

## 8. Funcionalidades del Dashboard

El dashboard mostrará:
- **Métricas del sistema:** Transacciones por segundo, volumen total procesado, porcentaje de fraude detectado.
- **Alertas:** Transacciones sospechosas, usuarios de alto riesgo.
- **Visualización:** Gráficos de comportamiento, tendencias de fraude, mapas geográficos.

## 9. Posibles extensiones

Para hacer el proyecto más avanzado se pueden agregar:
- Detección de fraude en tiempo real mediante streaming.
- Clustering de usuarios sospechosos.
- Análisis de grafos de transacciones.
- Scoring dinámico de riesgo.
