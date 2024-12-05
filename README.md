from selenium.common.exceptions import NoSuchElementException, ElementNotInteractableException
import time

verifica = True
while verifica:
    try:
        # Tenta encontrar o botão de "mais comentários"
        mais_comentarios = navegador.find_element(By.XPATH, 'XPATH_BOTAO_COMENTARIOS')

        # Rola a tela até o botão
        navegador.execute_script("arguments[0].scrollIntoView({block: 'center'});", mais_comentarios)
        time.sleep(1)  # Espera para garantir que o botão está visível

        # Tenta clicar no botão
        if mais_comentarios.is_displayed() and mais_comentarios.is_enabled():
            mais_comentarios.click()
            print("Botão clicado.")
            time.sleep(2)  # Espera os novos comentários carregarem
        else:
            print("Botão não está interagível.")
            raise ElementNotInteractableException

    except (NoSuchElementException, ElementNotInteractableException):
        # Caso o botão não seja encontrado ou não seja interagível, verifica se a tela já chegou ao final
        print("Botão não encontrado ou não interagível. Descendo a tela...")
        current_position = navegador.execute_script("return window.scrollY;")
        navegador.execute_script("window.scrollBy(0, 500);")  # Desce a tela para baixo
        time.sleep(2)
        new_position = navegador.execute_script("return window.scrollY;")

        # Se a posição não mudar, provavelmente estamos no final da página
        if current_position == new_position:
            print("Chegou ao final da página. Finalizando o loop.")
            verifica = False
