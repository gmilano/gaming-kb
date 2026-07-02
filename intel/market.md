# Mapa de mercado — Gaming AI

> Players, tamaños, oportunidades. Foco LATAM + posicionamiento Globant.
> Última actualización: 2026-07-02

## Tamaño de mercado global

| Segmento | Valor 2025 | Valor 2026 (proy.) | CAGR | Horizonte |
|----------|------------|-------------------|------|-----------|
| AI in Games (global) | $2.87B | $3.4B | 18.5% | 2025-2026 |
| AI in Gaming (amplio) | $4.54B | — | 33.57% | 2025-2035 → $81.19B |
| Generative AI in Gaming | $1.79B | — | 23.2% | 2026 |
| NPCs + Digital Humans segment | — | 28.6% del total AI gaming | — | 2026 |
| AI in Games (horizonte 2035) | — | — | 33.57% | → $81.19B |
| LATAM Gaming market | $25.70B | $28.04B | 9.12% | 2026-2034 |

**Dato clave**: el 87% de los estudios usa AI agents en sus workflows (Google Cloud survey, 615 developers, jun-jul 2025). El 50%+ de estudios AAA usa AI en alguna capacidad.

---

## Players globales — Engines y plataformas

| Empresa | Tipo | Fortaleza AI | Debilidad | Modelo |
|---------|------|-------------|-----------|--------|
| **Unity Technologies** | Engine propietario | Unity AI 6.2 integrado en editor (Assistant, Generators, Inference Engine). ML-Agents OSS 19.5k stars. | Controversia pricing 2023, recuperando confianza. Cerrado para AAA top-tier. | Revenue share + suscripción |
| **Epic Games (Unreal)** | Engine propietario | Faster adoption AAA. Metahuman AI. Aura agent (Ramen VR). Persona Device para Fortnite creator. | Curva de aprendizaje alta. Royalty 5% > $1M. | Royalty |
| **Roblox** | Plataforma UGC | AI Assistant en Studio (MCP). CUBE 3D (1.8B params) para generación de objetos. OpenGameEval OSS. Creators: $1B+/año via DevEx. | Audiencia mayormente juvenil. Limitado para juegos "serios". | Revenue share con creadores |
| **Microsoft (Xbox/Azure)** | Cloud + engine | Azure AI Services para gaming (Cognitive, OpenAI en Azure). Sponsor O3DE. Phi-3/4 modelos pequeños para on-device. | No tiene engine propio compeitivo. | Cloud usage |
| **NVIDIA** | GPU + SDK | ACE (Avatar Cloud Engine): ASR+NLP+TTS+animación NPC en tiempo real. RTX Neural Shading. Fork de Godot con path tracing (GDC 2026). | Dependencia de hardware NVIDIA. Caro para indie. | Hardware + SDK licensing |
| **Google DeepMind** | Research + cloud | Concordia (OSS simulación social). Gemini API para games. Gemma 3n on-device (demostrado en Godot). | No tiene engine/plataforma de distribución propia. | Cloud API |
| **Amazon/AWS** | Cloud + engine | O3DE (Apache 2.0). AWS Bedrock para LLMs en juegos. GameLift para multiplayer. | O3DE tiene adopción baja vs Unity/Unreal. | Cloud usage |
| **Inworld AI** | AI NPC platform | Personajes AI con voz, memoria, emoción. Partner de NVIDIA (Covert Protocol demo). | Plataforma cerrada, pricing por personaje. | SaaS |
| **Convai** | AI NPC platform | NPCs conversacionales con voz en tiempo real. Integración con Unity/Unreal/Godot. | Cerrado, pricing por llamada. | SaaS |

---

## Open Source — Ecosystem players

| Organización | Repos clave | Relevancia |
|-------------|-------------|------------|
| **Farama Foundation** | Gymnasium, PettingZoo | Estándar para entornos RL de juego |
| **Unity Technologies** | ml-agents (Apache 2.0) | Estándar para RL en Unity |
| **Heroic Labs** | Nakama (Apache 2.0) | Backend OSS más adoptado para juegos |
| **Stanford OVAL Lab** | generative_agents (Apache 2.0) | Arquitectura de referencia para NPCs |
| **Google DeepMind** | concordia (Apache 2.0) | Simulación social generativa |
| **DLR-RM** | stable-baselines3 (MIT) | RL algoritmos confiables |
| **O3DE Foundation (Linux Found.)** | o3de (Apache 2.0) | Engine AAA sin royalties |

---

## Mercado LATAM — Gaming + AI

### Tamaño y crecimiento
- Mercado gaming LATAM: **$25.7B (2025)** → $28.04B (2026), CAGR 9.12%
- LATAM: 1.12% del gasto global en AI (vs 6.6% del PIB global) → **brecha = oportunidad masiva**
- Mobile gaming dominante: infraestructura móvil en crecimiento, audiences en Brasil, México, Argentina, Colombia

### Drivers específicos LATAM
- **Brasil**: mayor mercado gaming LATAM, 100M+ gamers, regulación de iGaming legalizándose (2025)
- **México**: fuerte cultura gaming, industria iGaming creciente, talento tech disponible
- **Argentina**: comunidad dev muy activa (efecto devaluación → más freelancers/estudios), mercado de exportación
- **Colombia**: hub emergente para estudios indie y game studios medianos

### Oportunidades para Globant AI Studios

| Oportunidad | Por qué ahora | Fit Globant |
|-------------|--------------|-------------|
| **AI NPC para estudios LATAM** | Estudios locales no tienen budget para Inworld/Convai ($$$). OSS stack asequible. | Globant puede ofrecer implementación de stack OSS (Godot + LLM + generative agents) |
| **Backend gaming inteligente** | Nakama es OSS pero requiere customización con ML para diferenciarse. | Globant Gaming Studio + AI Studio: propuesta "Nakama + AI layer" |
| **iGaming + AI** | Brasil legalizó iGaming 2025. Mercado de $X billion en regulación/compliance + personalización. | Anti-fraud ML, recomendación de contenido, soporte con agentes |
| **QA automatizado con AI** | Estudios mid-size LATAM no tienen QA bots. Gap enorme vs AAA. | Productizing QA AI agent como servicio horizontal |
| **Analytics + churn prediction** | F2P mobile dominante en LATAM. Retención = survival. Churn prediction con GNNs. | Data + AI practice de Globant |
| **Exportación de juegos LATAM** | Estudios LATAM buscan diferenciarse en mercado global con AI. Globant como acelerador. | Co-inversión o servicio de AI integration |

### Competencia en LATAM
- **Accenture, Wipro**: presencia pero sin foco en gaming AI
- **Studios locales**: Aquiris (Brasil), Digital Sun (España), Luderia (México) — potenciales clientes
- **Startups AI gaming**: emergiendo pero sin capacidad de delivery enterprise

---

## Posicionamiento Globant AI Studios

### Propuesta de valor diferenciada
1. **Stack OSS end-to-end**: Godot/Nakama/SB3 + capa AI (sin lock-in de plataforma cerrada)
2. **Delivery LATAM**: costo y velocidad vs Accenture/Deloitte; calidad vs estudios locales
3. **AI Studios expertise**: combinación única de gaming domain + AI practice
4. **Proofs of concept rápidos**: patterns de `compose/patterns.md` → demos en semanas

### Cuentas objetivo (perfil)
- Estudios AAA/AA con presencia en LATAM (USA-based con operaciones latam)
- Plataformas iGaming entrando a Brasil/México
- Operadores de gaming online que necesitan anti-fraud + personalización
- Estudios indie/mid-size buscando AI para competir con AAA

---
*Fuentes: marketresearchfuture.com, snsinsider.com, agentmarketcap.ai, marketdataforecast.com, thomsonreuters.com/latam-ai, heroiclabs.com (2026-07-02)*
