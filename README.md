# PlatIOT Hub

PlatIOT-Hub er prosjektet for å sette opp en Raspberry Pi som en hub for datainnsamling og visualisering. Det inkluderer oppsett for:
- Mosquitto MQTT broker
- InfluxDB tidsseriedatabase
- Grafana for datavisualisering

## Instruksjoner

For å sette opp hubben på en Raspberry Pi, kjør følgende Ansible playbook:

```bash
ansible-playbook -i inventory.ini setup_raspberry_pi.yml
```

