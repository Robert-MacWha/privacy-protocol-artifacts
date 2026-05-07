## Artifacts

### Railgun
- https://github.com/Railgun-Privacy/circuits-v2/tree/main
- https://ipfs-lb.com/ipfs/QmUsmnK4PFc7zDp2cmC4wBZxYLjNyRgWfs5GNcJJ2uLcpU/circuits/01x02/

### Railgun PPOI
- https://github.com/Railgun-Privacy/circuits-ppoi/tree/main
- https://ipfs-lb.com/ipfs/QmZrP9zaZw2LwErT2yA6VpMWm65UdToQiKj4DtStVsUJHr/

### proving_key.bin & matrices.bin

Circom's `read_zkey` fn is quite slow (upward of 3s in release mode) so I've pre-generated the proving key and matrices for easier use.

```rust
let proving_key = ark_circom::ProvingKey<ark_bn254::Bn254>::deserialize_compressed(&mut R);
let matrices = SerializableConstraintMatrices<ark_bn254::Fr>::deserialize_compressed;

/// Serializable copy of `ConstraintMatrices<F>`
#[derive(Debug, Clone, CanonicalSerialize, CanonicalDeserialize)]
pub struct SerializableConstraintMatrices<F: Field> {
    /// The number of variables that are "public instances" to the constraint
    /// system.
    pub num_instance_variables: usize,
    /// The number of variables that are "private witnesses" to the constraint
    /// system.
    pub num_witness_variables: usize,
    /// The number of constraints in the constraint system.
    pub num_constraints: usize,
    /// The number of non_zero entries in the A matrix.
    pub a_num_non_zero: usize,
    /// The number of non_zero entries in the B matrix.
    pub b_num_non_zero: usize,
    /// The number of non_zero entries in the C matrix.
    pub c_num_non_zero: usize,

    /// The A constraint matrix. This is empty when
    /// `self.mode == SynthesisMode::Prove { construct_matrices = false }`.
    pub a: Matrix<F>,
    /// The B constraint matrix. This is empty when
    /// `self.mode == SynthesisMode::Prove { construct_matrices = false }`.
    pub b: Matrix<F>,
    /// The C constraint matrix. This is empty when
    /// `self.mode == SynthesisMode::Prove { construct_matrices = false }`.
    pub c: Matrix<F>,
}

impl<F: Field> From<ConstraintMatrices<F>> for SerializableConstraintMatrices<F> {
    fn from(matrices: ConstraintMatrices<F>) -> Self {
        Self {
            num_instance_variables: matrices.num_instance_variables,
            num_witness_variables: matrices.num_witness_variables,
            num_constraints: matrices.num_constraints,
            a_num_non_zero: matrices.a_num_non_zero,
            b_num_non_zero: matrices.b_num_non_zero,
            c_num_non_zero: matrices.c_num_non_zero,
            a: matrices.a,
            b: matrices.b,
            c: matrices.c,
        }
    }
}

impl<F: Field> From<SerializableConstraintMatrices<F>> for ConstraintMatrices<F> {
    fn from(matrices: SerializableConstraintMatrices<F>) -> Self {
        Self {
            num_instance_variables: matrices.num_instance_variables,
            num_witness_variables: matrices.num_witness_variables,
            num_constraints: matrices.num_constraints,
            a_num_non_zero: matrices.a_num_non_zero,
            b_num_non_zero: matrices.b_num_non_zero,
            c_num_non_zero: matrices.c_num_non_zero,
            a: matrices.a,
            b: matrices.b,
            c: matrices.c,
        }
    }
}

```
