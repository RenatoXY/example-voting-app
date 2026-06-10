# ==========================================
# IE1: Selección de Imagen Optimizada (Alpine)
# ==========================================
FROM python:3.11-alpine

# Directorio de trabajo en el contenedor
WORKDIR /app

# Instalar dependencias del sistema esenciales
RUN apk add --no-cache gcc musl-dev linux-headers

# Copiar e instalar dependencias de la app de forma eficiente
COPY requirements.txt /app/requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Copiar el código fuente
COPY . /app

# ==========================================
# IE3: Ajuste de Seguridad y Gobernanza (No-Root)
# ==========================================
# Creamos un usuario sin privilegios para que la app no corra como Root
RUN adduser -D appuser && chown -R appuser /app
USER appuser

# Puerto de escucha
EXPOSE 80

# Comando de ejecución preparado para alta disponibilidad/escalabilidad
CMD ["gunicorn", "app:app", "-b", "0.0.0.0:80", "--log-file", "-", "--access-logfile", "-", "--workers", "4"]