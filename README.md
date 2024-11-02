# teste
#!/bin/bash

endpoint_url="https://seu-endpoint.com/barcode"
filial_id="123"           # Defina o ID da filial
raspberry_id="RPi-01"     # Defina o ID do Raspberry Pi

while true; do
  # Captura a entrada do scanner até que Enter seja pressionado
  echo "Aguardando código de barras..."
  read barcode

  # Captura a data e hora atuais
  date_time=$(date +"%Y-%m-%d %H:%M:%S")

  # Envia o código de barras e outros dados para o endpoint
  response=$(curl -s -o /dev/null -w "%{http_code}" -X POST -H "Content-Type: application/json" \
    -d "{\"codigo_de_barras\":\"$barcode\", \"date_time\":\"$date_time\", \"filial_id\":\"$filial_id\", \"raspberry_id\":\"$raspberry_id\"}" \
    "$endpoint_url")

  # Verifica a resposta do servidor
  if [ "$response" -eq 200 ]; then
    echo "Dados enviados com sucesso!"
  else
    echo "Falha ao enviar. Código HTTP: $response"
  fi
done
