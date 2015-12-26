# Artigo - Inheritance
**Autor**: Matheus Jose Krumenauer Weber - [matheusjkweber](https://github.com/matheusjkweber)
**Data**: 1451171634158

#Herança
O conceito de herança é algo básico para qualquer linguagem que possua orientação a objetos, o mesmo serve para generalizar e especializar classes, re-utilizar código, bem como organizar todo o sistema.

Para entender o conceito de herança podemos imaginar que temos que modelar um sistema para acesso a uma biblioteca, onde serão cadastrados alunos e professores, ambos possuem muitos dados e métodos em comum, então para isso criaremos uma classe Usuario que concentrará os mesmos.

```
function Usuario(email,senha,ultimoAcesso,nome) { 
	var email = email;
	var senha = senha;
	var ultimoAcesso = ultimoAcesso;
	var nome = nome;

	this.getEmail = function () { 
		return email; 
	}; 

	this.getSenha = function () { 
		return senha; 
	};

	this.getUltimoAcesso = function () { 
		return senha; 
	};
}
```
Temos uma classe bem generalizada, agora criaremos a especialização da mesma em usuarios e professores.

```
function Aluno(matricula,curso) { 
	var matricula = matricula;
	var curso = curso;

	this.getMatricula = function () { 
		return matricula; 
	}; 

	this.getCurso = function () { 
		return curso; 
	};

}

function Professor(id,turno) { 
	var id = id;
	var turno = turno;

	this.getId = function () { 
		return id; 
	}; 

	this.getTurno = function () { 
		return turno; 
	};

}
```

Ao criarmos essas classes dessas maneiras evitaremos a repetição de código em duas classes transferindo tudo para uma classe pai(Usuario), fazendo com que a classe Aluno e Professor herdem esses atributos(e métodos) através de herança definida a seguir:

```
Aluno.prototype = new Usuario(); 
Professor.prototype = new Usuario();

```

