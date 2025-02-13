<script lang="ts">
    import { goto } from '$app/navigation'    
    
    class Coordenada {
        linha : number
        coluna : number
    }
    
    
    class EstadoJogo {
        posicaoPersonagem : Coordenada
        posicaoPersonagemB : Coordenada
        posicaoObjetivo : Coordenada
        mapa : number[][]
    }

    
    
    function inicializarJogo() : EstadoJogo {
        
        let personagemA: Coordenada = new Coordenada()
        personagemA.linha = 4
        personagemA.coluna = 0

        let personagemB: Coordenada = new Coordenada()
        personagemB.linha = 6
        personagemB.coluna = 0

        let objetivo : Coordenada = new Coordenada()
        objetivo.linha = 5
        objetivo.coluna = 9
        
        let mapa : number[][] = [[0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
                                 [0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
                                 [0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
                                 [0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
                                 [0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
                                 [1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
                                 [0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
                                 [0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
                                 [0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
                                 [0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
                                 [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
    ]
    
    let estado : EstadoJogo = new EstadoJogo()
    estado.posicaoPersonagem = personagemA
    estado.posicaoPersonagemB = personagemB
    estado.posicaoObjetivo = objetivo
    estado.mapa = mapa

    return estado;
    
}


function houveColisao(posicao : Coordenada, jogo : EstadoJogo) : boolean {
    return (posicao.linha < 0 || posicao.coluna < 0)
    || (posicao.linha >= jogo.mapa.length || posicao.coluna >= jogo.mapa[0].length)
    || jogo.mapa[posicao.linha][posicao.coluna] == 1
}

function onKeyDown(evento: { keyCode: any; }): void {
    if (vezDoJogadorA) {
        let novaPosicao = new Coordenada();
        novaPosicao.linha = jogo.posicaoPersonagem.linha;
        novaPosicao.coluna = jogo.posicaoPersonagem.coluna;

        switch (evento.keyCode) {
            case 38: // up / cima
                novaPosicao.linha--;
                break;
            case 37: // left / esquerda
                novaPosicao.coluna--;
                break;
            case 39: // right / direita
                novaPosicao.coluna++;
                break;
            case 40: // down / baixo
                novaPosicao.linha++;
                break;
            case 88: // x / passa a vez
                vezDoJogadorA = false;
                return;
        }

        if (!houveColisao(novaPosicao, jogo)) {
            jogo.posicaoPersonagem = novaPosicao;
        }

    } else {
        let novaPosicaoB = new Coordenada();
        novaPosicaoB.linha = jogo.posicaoPersonagemB.linha;
        novaPosicaoB.coluna = jogo.posicaoPersonagemB.coluna;

        switch (evento.keyCode) {
            case 38: // w / cima
                novaPosicaoB.linha--;
                break;
            case 37: // a / esquerda
                novaPosicaoB.coluna--;
                break;
            case 39: // d / direita
                novaPosicaoB.coluna++;
                break;
            case 40: // s / baixo
                novaPosicaoB.linha++;
                break;
            case 88: // x / passa a vez
                vezDoJogadorA = true;
                return;
        }

        if (!houveColisao(novaPosicaoB, jogo)) {
            jogo.posicaoPersonagemB = novaPosicaoB;
        }
    }

    // Verificação do Objetivo
    if (jogo.posicaoPersonagem.linha === jogo.posicaoObjetivo.linha && jogo.posicaoPersonagem.coluna === jogo.posicaoObjetivo.coluna) {
        alert("Parabéns 1º Participante, você chegou ao objetivo");
        goto("/");
    } else if (jogo.posicaoPersonagemB.linha === jogo.posicaoObjetivo.linha && jogo.posicaoPersonagemB.coluna === jogo.posicaoObjetivo.coluna) {
        alert("Parabéns 2º Participante, você chegou ao objetivo");
        goto("/");
    }
}

    let jogo: EstadoJogo = inicializarJogo();
    let vezDoJogadorA: boolean = true;


</script>

<div class="content">
    <h1 class=titulo>MOVIMENTE O PERSONAGEM (CARRO) ATÉ O OBJETIVO FINAL (QUADRADO AMARELO)</h1>

    <table>
        {#each jogo.mapa as linha, i}
            <tr>
                {#each linha as celula, j}
                    {#if i == jogo.posicaoPersonagem.linha && j == jogo.posicaoPersonagem.coluna}
                        <td class="celula personagem"></td>
                    {:else if i == jogo.posicaoPersonagemB.linha && j == jogo.posicaoPersonagemB.coluna}
                        <td class="celula personagemb"></td>
                    {:else if i == jogo.posicaoObjetivo.linha && j == jogo.posicaoObjetivo.coluna}
                        <td class="celula objetivo"></td>
                    {:else if jogo.mapa[i][j] == 0}
                        <td class="celula"></td>
                    {:else if jogo.mapa[i][j] == 1}
                        <td class="celulan"></td>
                    {:else if jogo.mapa[i][j] == 2}
                        <td class="celula objetivo icon"></td>
                    {/if}
                {/each}
            </tr>
        {/each}
    </table>

    <br />

    <a class="menuj" href="/">Voltar ao Menu</a>

</div>



<svelte:window on:keydown|preventDefault={onKeyDown} />

