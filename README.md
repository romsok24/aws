Projekt na potrzeby stacka AWS Example Corp.


### Opis
Projekt mający na celu automatyzację zadań związanych z obsługą infrastruktury chmurowej AWS w Example Corp.
Obecnie obsługiwana funkcjonalność:
- pełna obsługa Jenkinsa ( SaaC )
- budowa i suuswanie instnacji AWS EC2
- tagowanie i wbór typu instancji
- konfiguracja klienta DNS do obsługi serwera na kontrolerze AD Example Corp
- dodanie  certyfikatu root CA do listy zaufanych certyfikatów

### Użycie
a) eksport koniecznych zmiennych środowiskowych - feel3 jeśli chcesz to zrobić przez Jenkinsa lub zmienną w ansible  :-)  
```
export AWS_ACCESS_KEY_ID="AK123"
export AWS_SECRET_ACCESS_KEY="abc123"
```

b) playbook invoke method example:
```
ansible-playbook -i ec2_inventory.py chnura_buduj.yml --tags "yyyyyyy" -e instance_ids="i-xxxxxxxxxxxx"
```

### Autor - informacje kontaktowe
Author contact information: sok.r@wp.pl

### Licencja
This code is subjected to Example Corp intellectual property disclaimers