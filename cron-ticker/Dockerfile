# ------------------------------------------------------------------------
# ETAPA DE DEPENDENCIAS
# ------------------------------------------------------------------------

# imagen base
FROM node:19.2-alpine3.16 as deps

# cd app
WORKDIR /app

# COPY source dest 
COPY package.json ./

# Instalar dependencias
RUN npm install

# ------------------------------------------------------------------------
# ETAPA DE TESTING Y CONSTRUCCIÓN
# ------------------------------------------------------------------------
FROM node:19.2-alpine3.16 as builder

WORKDIR /app
    
COPY --from=deps /app/node_modules ./node_modules

# COPY source dest
# COPY app.js ./
COPY . .

# Realizar testing
RUN npm run test

# ------------------------------------------------------------------------
# DEPENDENCIAS DE PRODUCCIÓN 
# ------------------------------------------------------------------------

FROM node:19.2-alpine3.16 as prod-deps

WORKDIR /app

COPY package.json ./

RUN npm install --prod

# ------------------------------------------------------------------------
# RUNNER 
# ------------------------------------------------------------------------
FROM node:19.2-alpine3.16 as runner

WORKDIR /app

COPY --from=prod-deps /app/node_modules ./node_modules

COPY app.js ./
COPY tasks/ ./tasks

# Comando run de la imagen
CMD [ "node", "app.js" ]

