import time
from selenium.common.exceptions import NoSuchElementException

verifica = True
while verifica:
    try:
        # Localizar o elemento
        mais_comentarios = navegador.find_element(By.XPATH, "/html/body/div[1]/div/div[1]/div[1]/div/div[3]/div/div[1]")
        
        # Verificar se o elemento está visível na tela
        is_in_viewport = navegador.execute_script(
            "var elem = arguments[0];" 
            "var bounding = elem.getBoundingClientRect();" 
            "return (bounding.top >= 0 && bounding.left >= 0 && bounding.bottom <= (window.innerHeight || document.documentElement.clientHeight) && bounding.right <= (window.innerWidth || document.documentElement.clientWidth));",
            mais_comentarios
        )
        
        if is_in_viewport:
            print("Elemento visível na tela")
            # Tentar clicar
            mais_comentarios.click()
            print("Clicou no botão!")
            time.sleep(2)
        else:
            # Rolar para localizar o elemento
            print("Elemento fora da tela, rolando...")
            navegador.execute_script("arguments[0].scrollIntoView({block: 'center'});", mais_comentarios)
            time.sleep(1)
    
    except NoSuchElementException:
        print("Elemento não encontrado, encerrando loop")
        verifica = False
