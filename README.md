import time
from selenium.common.exceptions import NoSuchElementException

verifica = True
while verifica:
    try:
        # Localizar o elemento
        mais_comentarios = navegador.find_element(By.XPATH, "/html/body/div[1]/div/div[1]/div[1]/div/div[3]/div/div[1]")

        # Verificar se o elemento está visível na tela
        is_in_viewport = navegador.execute_script(
            """
            var elem = arguments[0];
            var bounding = elem.getBoundingClientRect();
            return (
                bounding.top >= 0 &&
                bounding.left >= 0 &&
                bounding.bottom <= (window.innerHeight || document.documentElement.clientHeight) &&
                bounding.right <= (window.innerWidth || document.documentElement.clientWidth)
            );
            """,
            mais_comentarios
        )

        if is_in_viewport:
            print("Elemento visível na tela")
            # Tentar clicar
            mais_comentarios.click()
            print("Clicou no botão!")
            time.sleep(2)
        else:
            # Rolar manualmente para evitar resetar a posição para o mesmo local
            print("Elemento fora da tela, rolando mais para baixo...")
            navegador.execute_script("window.scrollBy(0, 500);")
            time.sleep(1)
    
    except NoSuchElementException:
        print("Elemento não encontrado, verificando se chegou ao final da página.")
        # Verificar se estamos no final da página
        current_position = navegador.execute_script("return window.scrollY;")
        navegador.execute_script("window.scrollBy(0, 500);")
        time.sleep(2)
        new_position = navegador.execute_script("return window.scrollY;")
        if current_position == new_position:
            print("Chegou ao final da página. Encerrando loop.")
            verifica = False
