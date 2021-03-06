<properties
   pageTitle="Polimorfismo na estrutura Reliable Actors | Microsoft Azure"
   description="Crie hierarquias de interfaces .NET e tipos na estrutura Reliable Actors para reutilizar a funcionalidade e as definições da API."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/08/2016"
   ms.author="seanmck"/>

# Polimorfismo na estrutura Reliable Actors

A estrutura Reliable Actors simplifica a programação de sistemas distribuídos. Ela faz isso permitindo que você compile seu serviço usando muitas das mesmas técnicas que você usaria no design orientado a objeto. Uma dessas técnicas é o polimorfismo, que permite que tipos e interfaces herdem de pais mais generalizados. A herança na estrutura de Reliable Actors geralmente segue o modelo de .NET com algumas restrições adicionais.

## Interfaces

A estrutura de Reliable Actors exige que você defina pelo menos uma interface a ser implementada pelo seu tipo de ator. Essa interface é usada para gerar uma classe proxy que pode ser usada pelos clientes para se comunicar com seus atores. Interfaces podem herdar de outras interfaces, desde que cada interface implementada por um tipo de ator e todos os seus pais derivem, por fim, de IActor. IActor é a interface base definida pela plataforma para atores. Assim, o exemplo de polimorfismo clássico usando formas pode parecer semelhante ao seguinte:

![Hierarquia de interface de atores de forma][shapes-interface-hierarchy]


## Tipos

Você também pode criar uma hierarquia de tipos de ator, derivados da classe base de ator fornecida pela plataforma. Para atores com estado, da mesma forma, você pode criar uma hierarquia de tipos de estado. No caso de formas, você pode ter um tipo de base `Shape` com um tipo de estado de `ShapeState`.

    public abstract class Shape : Actor<ShapeState>, IShape
    {
        ...
    }

Subtipos de `Shape` podem usar subtipos de `ShapeType` para armazenar propriedades mais específicas.

    [ActorService(Name = "Circle")]
    public class Circle : Shape, ICircle
    {
        private CircleState CircleState => this.State as CircleState;

        public override ShapeState InitializeState()
        {
            return new CircleState();
        }

        [Readonly]
        public override Task<int> GetVerticeCount()
        {
            return Task.FromResult(0);
        }

       [Readonly]
       public override Task<double> GetArea()
       {
           return Task.FromResult(
               Math.PI*
               this.CircleState.Radius*
               this.CircleState.Radius);
       }

       ...
    }

Observe o atributo `ActorService` no tipo de ator. Esse atributo informa o SDK do Service Fabric do Azure que ele deverá criar automaticamente um serviço de hospedagem de atores desse tipo. Em alguns casos, você poderá criar um tipo base que destina-se somente para o compartilhamento de recursos com subtipos e nunca será usado para criar uma instância de atores concretos. Nesses casos, você deve usar a palavra-chave `abstract` para indicar que você nunca criará um ator com base nesse tipo.


## Próximas etapas

- Consulte [Como a estrutura Reliable Actors aproveita a plataforma do Service Fabric](service-fabric-reliable-actors-platform.md) para oferecer estado consistente, escalabilidade e confiabilidade.
- Saiba mais sobre o [ciclo de vida do ator](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png

<!---HONumber=AcomDC_0309_2016-->