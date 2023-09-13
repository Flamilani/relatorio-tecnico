# Relatório Técnico: Elaboração de Solução em Situações de Incerteza
### Introdução
<p>Este relatório descreve o processo de elaboração de solução para enfrentar situações de incerteza em projetos tecnológicos. Em contextos em que os requisitos não estão totalmente definidos ou podem mudar, a abordagem flexível e iterativa é essencial para o sucesso do projeto.</p>

4 tipos de incerteza são:
-	Variação
- Incerteza prevista
-	Incerteza imprevista
-	Caos

### 1. Variação
Variação refere-se a um pequeno grau de mudança no cronograma do projeto. Por exemplo, você pode precisar gerenciar pequenos atrasos se os membros da equipe estiverem doentes ou se precisar preparar documentos adicionais para as partes interessadas.

### 2. Incerteza Prevista
As incertezas previstas são aquelas que você pode identificar e preparar. Diferentemente das pequenas mudanças trazidas pela variação, as incertezas previstas são eventos maiores que podem exigir gerenciamento de riscos e planejamento de contingência

### 3. Incerteza Imprevista
Incertezas imprevistas são eventos que você não pode antecipar ou que você considera tão improváveis que você não precisa criar um plano de contingência para resolvê-los. Esses tipos de incerteza são comuns em projetos de tecnologia ou naqueles que se concentram em mercados incertos.

### 4. Caos
Às vezes, você não pode esclarecer os planos no início de um projeto, talvez porque o mercado está mudando rapidamente. Na verdade, você pode achar que as expectativas que você teve no início do projeto mudam completamente à medida que o trabalho avança.


### Testes de unidade / integração

<p>**_Angular Testing Library_** é uma biblioteca da família [DOM Testing Library](https://testing-library.com/docs/dom-testing-library/intro) que tem como propósito oferecer uma abstração para testarmos componentes de UI de uma maneira mais voltada para o usuário.</p> 
<p>Usando o framework de teste Jasmine e o Angular Testing Library, pode criar testes de unidade/integração para o componente. Certifique-se de testar os diferentes cenários e manter seus testes atualizados.</p>

```
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { MeuComponenteComponent } from './meu-componente.component';

describe('MeuComponenteComponent', () => {
  let component: MeuComponenteComponent;
  let fixture: ComponentFixture<MeuComponenteComponent>;

  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [MeuComponenteComponent]
    });

    fixture = TestBed.createComponent(MeuComponenteComponent);
    component = fixture.componentInstance;
  });

  it('should create', () => {
    expect(component).toBeTruthy();
  });

  it('should set and get data', () => {
    const testData = [1, 2, 3];
    component.setData(testData);
    const data = component.getData();
    expect(data).toEqual(testData);
  });
});
```

Como aplicar os princípios de abstração, acoplamento, extensibilidade e coesão em um projeto Angular usando componentes e serviços.

### Abstração:
A abstração envolve a criação de abstrações para simplificar a complexidade e ocultar detalhes desnecessários. Vamos criar um serviço que abstrai o acesso a uma API fictícia para buscar dados.

```
// service/dados.service.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class DadosService {
  private apiUrl = 'https://api.example.com/data';

  constructor(private http: HttpClient) {}

  getData(): Observable<any[]> {
    return this.http.get<any[]>(this.apiUrl);
  }
}
```

Aqui, criamos uma abstração para acessar os dados. Se a URL ou a forma como os dados são obtidos mudar, só precisamos fazer alterações no serviço DadosService.

### Acoplamento:
Para manter um baixo acoplamento, evite que os componentes dependam fortemente uns dos outros. Use serviços para desacoplar a lógica de negócios dos componentes.

```
// component/meu-componente.component.ts
import { Component, OnInit } from '@angular/core';
import { DadosService } from '../service/dados.service';

@Component({
  selector: 'app-meu-componente',
  templateUrl: './meu-componente.component.html',
  styleUrls: ['./meu-componente.component.css']
})
export class MeuComponenteComponent implements OnInit {
  dados: any[] = [];

  constructor(private dadosService: DadosService) {}

  ngOnInit() {
    this.dadosService.getData().subscribe(data => {
      this.dados = data;
    });
  }
}
```

### Extensibilidade:
Para tornar o código extensível, você pode usar herança ou interfaces para adicionar novos recursos sem modificar o código existente. Aqui, vamos adicionar um novo serviço NovosDadosService que implementa a mesma interface que DadosService.

```
// service/novos-dados.service.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { DadosService } from './dados.service';

@Injectable({
  providedIn: 'root'
})
export class NovosDadosService implements DadosService {
  private apiUrl = 'https://api.example.com/new-data';

  constructor(private http: HttpClient) {}

  getData(): Observable<any[]> {
    return this.http.get<any[]>(this.apiUrl);
  }
}
```

<br />

Gerenciar situações de incerteza é uma parte importante do desenvolvimento de software, pois nem sempre é possível prever todos os cenários ou erros que podem ocorrer. Vou mostrar como você pode incorporar tratamento de erros e situações de incerteza em seu código Angular usando try-catch, manipulação de erros e feedback ao usuário.

1. Tratamento de Erros:

Para gerenciar situações de incerteza, você pode usar blocos try-catch em seu código. Por exemplo, ao fazer uma solicitação HTTP para buscar dados, você pode capturar erros de rede ou erros no servidor:

```
import { Component, OnInit } from '@angular/core';
import { DadosService } from '../service/dados.service';

@Component({
  selector: 'app-meu-componente',
  templateUrl: './meu-componente.component.html',
  styleUrls: ['./meu-componente.component.css']
})
export class MeuComponenteComponent implements OnInit {
  dados: any[] = [];
  erro: string = '';

  constructor(private dadosService: DadosService) {}

  ngOnInit() {
    this.dadosService.getData().subscribe(
      data => {
        this.dados = data;
      },
      error => {
        this.erro = 'Ocorreu um erro ao buscar os dados.';
        console.error(error); // Log de erro para depuração
      }
    );
  }
}
```
Aqui, usamos um bloco try-catch implícito ao tratar erros com o método subscribe.

2. Feedback ao Usuário:

É importante fornecer feedback ao usuário quando ocorrerem erros. Neste exemplo, atribuímos uma mensagem de erro à variável erro, que pode ser exibida no template para informar o usuário sobre o problema.

```
<div *ngIf="erro" class="error-message">
  {{ erro }}
</div>
```

3. Tratamento de Erros em Serviços:

Em seu serviço, você também pode tratar erros antes de propagá-los para os componentes. Aqui está um exemplo de como isso pode ser feito no serviço DadosService:

```
import { Injectable } from '@angular/core';
import { HttpClient, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';

@Injectable({
  providedIn: 'root'
})
export class DadosService {
  private apiUrl = 'https://api.example.com/data';

  constructor(private http: HttpClient) {}

  getData(): Observable<any[]> {
    return this.http.get<any[]>(this.apiUrl).pipe(
      catchError(this.handleError)
    );
  }

  private handleError(error: HttpErrorResponse) {
    let errorMessage = 'Ocorreu um erro na solicitação.';
    if (error.error instanceof ErrorEvent) {
      errorMessage = `Erro do cliente: ${error.error.message}`;
    } else {
      errorMessage = `Erro do servidor: ${error.status}, ${error.message}`;
    }
    console.error(errorMessage);
    return throwError(errorMessage);
  }
}
```

Aqui, usamos o operador catchError para interceptar erros da solicitação HTTP e fornecer uma mensagem de erro adequada. Isso evita que os erros cheguem aos componentes sem tratamento.

Lidar com situações de incerteza dessa maneira ajuda a tornar seu aplicativo mais robusto e fornece aos usuários uma melhor experiência, informando-os sobre problemas e fornecendo feedback adequado. Certifique-se de personalizar o tratamento de erros de acordo com os requisitos e a complexidade do seu aplicativo.

### Conclusão
A elaboração de solução em situações de incerteza é um processo iterativo e adaptativo que envolve compreensão, criatividade e colaboração. Ao seguir os passos descritos neste relatório e estar disposto a ajustar a solução à medida que novas informações surgirem, é possível enfrentar desafios de forma eficaz e alcançar resultados satisfatórios.

### Referências bibliográficas:
https://andrewrosario.medium.com/escrevendo-testes-eficientes-de-verdade-no-angular-263a9d03cecf <br/>
https://fm2s.com.br/blog/como-gerenciar-a-incerteza-do-projeto-planeje-o-desconhecido

