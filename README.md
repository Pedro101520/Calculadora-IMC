from selenium.common.exceptions import NoSuchElementException, ElementNotInteractableException
import time

verifica = True
while verifica:
    try:
        # Localiza o botão "Mais comentários"
        mais_comentarios = navegador.find_element(By.XPATH, 'XPATH_BOTAO_COMENTARIOS')

        # Rola até o botão estar visível
        while not mais_comentarios.is_displayed():
            navegador.execute_script("arguments[0].scrollIntoView({block: 'center'});", mais_comentarios)
            time.sleep(1)

        # Clica no botão
        mais_comentarios.click()
        print("Botão clicado.")
        time.sleep(2)  # Espera o carregamento dos novos comentários

    except NoSuchElementException:
        print("Botão não encontrado. Descendo a página...")
        current_position = navegador.execute_script("return window.scrollY;")
        navegador.execute_script("window.scrollBy(0, 500);")  # Rola para baixo
        time.sleep(2)
        new_position = navegador.execute_script("return window.scrollY;")

        # Se a posição não mudar, estamos no final da página
        if current_position == new_position:
            print("Chegou ao final da página. Finalizando...")
            verifica = False

    except ElementNotInteractableException:
        print("Botão não está interagível. Continuando a rolar...")
        navegador.execute_script("window.scrollBy(0, 500);")
        time.sleep(2)
